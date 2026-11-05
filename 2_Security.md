# CompTIA+ Security Concepts: Applied

Since the beginning of August, I decided to embark on the Cybersecurity Path as my specialization. I wanted a good foundation, so I bought this large brick of a textbook, the Security+ Study Guide by Mike Chapple & David Siedl. I'm about 350 pages in at the time of writing, and have learned a host of concepts such as Network Security, AppSec, Endpoint Security, upon many, many others. Despite the interesting and valuable information this book has provided me, I have yet to apply any of the information presented in the book. 

So I decided this project would be a perfect opportunity to apply these concepts along with learning how to properly configure, deploy, and document various changes, security technologies and updates.. Alongside also learn and practice the Linux CLI.

Firstly though, I wanted to setup SSH (Secure Shell) so I don't need to be hunched over in my closet to work on the system. 

I learned that ```apt``` is the package manager for Debian.
To install a package, use ```install```, the SSH package is called "open-sshserver."

Also all together, the command to install SSH is: ```apt install open-sshserver```
After confirming ```-y```, it automatically installs.

Then the last step was to turn on the OpenSSH server with ```systemctl``` ("System Control"), which allows you to control resources of the system.
I executed ```systemctl enable ssh```

After confirming SSH was running with ```systemctl status ssh```, I went ahead and added myself as a user using ```adduser```, after creating a password I tested it by creating an SSH session from my main machine, it worked!

I then went ahead and logged into my router's web interface to give my main machine and the server a static IP address, as I didn't want to go and hunt for the new IP address DHCP gave it every so often. 

Now it was time for the fun part! Firewall configuration! (specifically with UFW, Uncomplicated Firewall)

I planned on this server using my domain ```*.jmoo.dev``` to communicate with the outside world and my devices, so I went with a baseline Deny in Deny out firewall schema, as it had no reason to communicate with anyone from my network.

I did this through 

