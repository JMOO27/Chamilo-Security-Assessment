# Phase 3: Remote Access

With Chamilo successfully deployed, I wanted to ensure I could continue my research without being restricted to my home network. This meant finding a way to securely access both the Chamilo web application and my server's SSH service over the Internet.

There are several ways to accomplish this, but some methods, such as traditional port forwarding, expose services directly to Internet traffic. This increases the attack surface by making those services reachable by automated scanners and potentially malicious actors.

During my research, I discovered **Cloudflare Tunneling**, a solution that allows remote access without requiring inbound ports to be opened on my router.

The Cloudflare Tunnel operates through a lightweight daemon called `cloudflared`, which runs on the host system. Rather than accepting unsolicited inbound connections from the Internet, the daemon establishes outbound connections to Cloudflare's infrastructure. When a remote user attempts to access a configured application, their request first reaches Cloudflare. Cloudflare then forwards the request through the existing tunnel to the appropriate service running on my server. The response travels back through the tunnel to Cloudflare, which delivers it to the user.

This architecture eliminates the need for traditional port forwarding and prevents the server's services from being directly exposed through the router's public IP address.

In addition to encrypted tunnel connections, Cloudflare provides Zero Trust access controls that can restrict who is permitted to reach an application.

For my SSH service, I configured `ssh.jmoo.com` with a Cloudflare Access policy requiring users to authenticate before their connection can proceed.

This adds an authentication layer in front of SSH, helping prevent unauthorized users from reaching the service directly through the tunnel. SSH's own authentication mechanisms remain in place.

Despite this though, hardening of all surfaces is still required.