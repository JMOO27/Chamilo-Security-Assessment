# From Theory to Practice: Applying Security+ Concepts to My Debian Homelab

## 1. Introduction: Moving Beyond the Textbook

At the beginning of August, I decided to pursue cybersecurity as my specialization. I wanted to establish a strong technical foundation, so I picked up a rather intimidating textbook: the *CompTIA Security+ Study Guide* by Mike Chapple and David Seidl.

At the time of writing, I'm approximately 480 pages into the book. So far, I've explored a variety of topics, including network security, IAM, endpoint security, cryptography, and many others.

While the textbook has provided me with a considerable amount of valuable information, I realized something important: **learning about security concepts is one thing, but implementing them is another.**

I wanted an opportunity to apply what I had been studying to an actual system.

That's where my Debian homelab comes in.

My goal was to take an existing machine and configure it as a Linux server, applying the principles I had learned throughout my Security+ studies.

Along the way, I also wanted to improve my familiarity with the Linux command-line interface (CLI), system administration, service management, and technical documentation.

## 2. Establishing Remote Administration with SSH

My first objective was to establish remote access to the server.

For context, my server is physically located in a closet. While I could connect a keyboard and monitor whenever I wanted to administer it, I would much rather avoid being hunched over in there every time I needed to make a configuration change.

Fortunately, Linux provides a convenient solution: SSH (Secure Shell).

SSH is a protocol that enables encrypted remote communication between systems. It is commonly used by administrators to manage servers without requiring physical access.

### Installing and Enabling OpenSSH

Because my server runs Debian, I used its APT package management system to install OpenSSH.

```bash
sudo apt update
sudo apt install openssh-server
```

Once installed, I used `systemctl` to enable and start the SSH service.

```bash
sudo systemctl enable --now ssh
```

`systemctl` is a command-line utility used to manage services controlled by systemd, Debian's service manager.

The `enable` option configures SSH to start automatically during boot, while `--now` starts the service immediately.

To verify that the service was running, I executed:

```bash
sudo systemctl status ssh
```

### Creating a User and Testing Connectivity

With SSH operational, I created a user account using:

```bash
sudo adduser <username>
```

After configuring a password, I attempted to establish an SSH connection from my main workstation:

```bash
ssh <username>@<server-ip>
```

Success! I could now administer my Debian server remotely, without having to physically access the machine.

## 3. Configuring DHCP Reservations

Before moving on to the firewall, I wanted to address one potential administrative inconvenience: changing IP addresses.

By default, devices on my home network receive their IP addresses through DHCP (Dynamic Host Configuration Protocol).

While DHCP simplifies network administration, addresses assigned to devices may change over time.

For a server I planned to administer remotely, I wanted a predictable network address.

I accessed my router's administrative interface and configured DHCP reservations for my Debian server and the devices I intended to use for administration.

This allows those devices to continue using DHCP while consistently receiving their reserved IP addresses.

Aside from making remote administration more convenient, these reservations would also become useful when defining firewall rules based on source IP addresses.

## 4. Implementing a Default-Deny Firewall Policy

With remote administration configured, I turned my attention to controlling network traffic.

This was particularly interesting because it allowed me to apply several concepts from my Security+ studies, including:

- The principle of least privilege
- Network traffic filtering
- Stateful firewalls
- Attack surface reduction

For this project, I decided to use UFW (Uncomplicated Firewall), a frontend for managing Linux firewall rules.

### Planning My Network Security Policy

My long-term plan was to make selected services available through my domain, `*.jmoo.dev`, using Cloudflare Tunnel running inside a Docker container.

Rather than exposing individual services directly to the internet through router port forwarding, the tunnel would establish outbound connectivity to Cloudflare.

I wanted to restrict the server's network communication to connections that were explicitly necessary.

To accomplish this, I chose a **default-deny policy for both incoming and outgoing traffic**.

Under this model, traffic is denied unless an explicit firewall rule permits it.

This approach aligns with the principle of least privilege: systems should only have the access necessary to perform their intended functions.

### Configuring the Default Policies

I used the following commands to establish my baseline firewall policies:

```bash
sudo ufw default deny incoming
sudo ufw default deny outgoing
```

These policies establish the default behavior for traffic that does not match an existing rule.

However, a default-deny configuration also introduces an important consideration: necessary communication must be explicitly permitted.

For example, I still needed SSH access, DNS resolution, and access to software repositories for system updates.

### Allowing SSH from Trusted Devices

My first exceptions were for my primary workstation and laptop.

Rather than allowing SSH connections from any device on the network, I restricted access to the IP addresses of the systems I intended to use for administration.

```bash
sudo ufw allow in from 192.168.1.63 to any port 22 proto tcp
sudo ufw allow in from 192.168.1.254 to any port 22 proto tcp
```

These rules permit incoming TCP connections to port 22, the default port used by SSH, from the specified addresses.

This provides an additional layer of access restriction by limiting which systems can initiate SSH connections to the server.

However, IP-based filtering does not replace secure authentication. It is simply one component of a broader defense-in-depth strategy.

**Important:** When implementing these rules on a remotely administered system, the necessary SSH exceptions should be configured and verified before enabling the firewall to avoid accidentally locking yourself out.

### Allowing DNS Resolution

Next, I needed to ensure that the server could resolve domain names.

DNS (Domain Name System) translates domain names into IP addresses, allowing the server to locate external resources such as software repositories.

My router, located at `192.168.1.1`, serves as the DNS resolver for my local network.

I permitted outbound DNS queries to that address:

```bash
sudo ufw allow out to 192.168.1.1 port 53 proto udp
```

DNS commonly uses UDP port 53, although TCP port 53 may also be required in certain circumstances.

Restricting DNS traffic to my designated resolver helps limit where the server can send ordinary DNS queries.

### Allowing System Updates

An equally important part of maintaining a secure system is keeping its software up to date.

Because I had configured a default-deny outgoing policy, I needed to explicitly permit connections used for retrieving software updates.

I created the following rules:

```bash
sudo ufw allow out to any port 443 proto tcp
sudo ufw allow out to any port 80 proto tcp
```

These rules allow outbound HTTPS and HTTP traffic, respectively.

Although the rules are broader than restricting traffic to specific repository destinations, they provide the connectivity needed for normal package-management operations.

This was also a useful lesson: **a default-deny policy doesn't automatically create a secure system. Its effectiveness depends on the exceptions that are permitted.**

### Understanding Stateful Firewall Behavior

One concept from my Security+ studies that became particularly relevant during this process was stateful packet inspection.

UFW uses connection tracking through the Linux firewall subsystem.

This means the firewall can recognize traffic belonging to established connections and permit the corresponding responses.

For example, when my server initiates an allowed HTTPS connection, the returning packets are recognized as part of that connection.

As a result, I do not need to create a separate inbound rule for every permitted outbound connection.

This is different from a purely stateless filtering approach, where decisions are made without tracking connection state.

## 5. Validating the Firewall Configuration

Configuring firewall rules is only one part of the process. I also wanted to ensure that the rules behaved as intended.

A configuration that appears secure on paper may behave differently once other components, such as Docker, are introduced.

To inspect my current firewall configuration, I can use:

```bash
sudo ufw status verbose
```

To inspect services listening for network connections:

```bash
sudo ss -tulpen
```

For the purposes of testing, I temporarily removed my laptop from communicating with the server.

I then attempted to SSH into it, it continuously hanged until the request timed out. Verified!

To verify HTTP, HTTPS, and DNS worked, I originally attempted using:

```bash
ping google.com
```

But the request timed out unexpectedly. I reanalyzed the my rules, everything seemed correct. But a google search revealed ping runs on ICMP (Internet Control Message Protocol) not HTTP or HTTPS. 

So I attempted to curl google.com instead:

```bash
sudo curl google.com
```

And got the HTML source of the page, success! To test I could successfully recieve patches:

```bash
sudo apt update
```

And saw the system was successfully able to reach and download patches. Success!!

## 6. Lessons Learned and Next Steps

This project has already helped me appreciate the difference between understanding a security concept and implementing it.

Concepts such as least privilege, stateful filtering, and attack surface reduction seem relatively straightforward when described in a textbook.

However, implementing them requires accounting for the way operating systems, applications, and networking components interact.

A seemingly simple decision, such as denying outgoing traffic by default, requires understanding the legitimate network communications a server needs to function.

My next steps include strengthening SSH authentication, further server hardening to CIS (Center for Internet Security) benchmarks, proper documentation of changes, possible VLAN configurations and much more.