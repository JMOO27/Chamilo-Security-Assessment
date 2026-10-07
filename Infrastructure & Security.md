# Phase #1: Infrastructure 10/4 - 10/6

This project originated from me wanting a homelab and also for my senior capstone project. I decided, why not kill two birds with one stone? I could use a machine to both host the LMS I'm going to perform a security assessment on, as well as use it to self-host services and projects, such as VPNs, file servers, a DHCP server, a web server, and whatever else I may want to work with.

Originally, I had planned to gather and buy parts through various marketplaces, stores, and online retailers. Unfortunately, big AI has beat me to it, the prices for most basic parts are inflated way too high, entirely beating my humble $500 budget. Of course I had the option to endure the inflated prices and buy the parts needed, but I'd rather wait until the parts supply catches up so I can buy something substantial, something a bit more *"bang for your buck."* 

Regardless, I still needed a system to tinker with, and I'd rather not use my main machine and perhaps mess with something I won't know how to fix, leaving me stranded. Instead I went with plan B, in my closet I had entirely forgotten about a late 2015 IMac Desktop, still worked perfectly fine, just a bit (very!) obsolete.

The specs are as follows:
* 3.1 Ghz Quad-Core Intel Core i5
* 8 GB 1867 MHz DDR3
* Intel Iris Pro Graphics 6200 1536MB
* And just about the slowest 1TB hard drive possible!

After powering her on for the first time in years, she surprisingly worked as intended. Although the HDD made the entire system extremely sluggish, that and MacOS consuming more than 3/4ths of the RAM on idle. I did the only sensible thing in this situation, I installed Linux. 

Specifically, I installed Debian 13 XCFE. I used Rufus on my laptop to format a thumbdrive for the hardware to boot from. I tested an image that came with the drive and confirmed the hardware was perfectly capable of running Linux. 

The next problem I had tackle was the HDD, I *could* open up the desktop by removing adhesive from the 21 inch screen, remove the HDD and install a newer SATA SSD, then reapply the adhesive and pray the display doesn't fall off later on. But compared to the lazy solution, where I use the desktops USB 3.0 interface with a SATA USB adapter, I achieve essentially the same result with substantially less risk. So I ordered a Patriot 256GB SATA SSD drive, and a Sabrent SSD enclosure & USB adapter. After formatting the new drive and putting it into enclosure, I used the drive to boot into the installation.

Now this is where the first roadblock began, after I had plugged in everything needed, I booted into the install screen and not a minute into the process I ran into a wall of multiple fatal I/O disk errors. Of course, there was some sort of issue with the SSD installation, the question was what? After waiting a few mintues for the hung OS installation process to figure it out, I eventually force powered down the machine. I did some basic troubleshooting, was the USB going into either end properly seated? Was the SSD properly seated? Does it get recognized on my laptop and main machine? After verifying all was well, I decided perhaps it was the USB interface that was bad, it was an old machine afterall. I plugged it into the USB interface next to the original, and did the entire boot sequence again, and like a charm, I saw the installation manager process.

All I had to do next, is simply choose the basic options needed, and soon enough I booted into the login screen, from there I researched and did a couple of health checks against the SSD; we're all in the clear. By that point, all I had to do was connect the server via ethernet to my managed ethernet switch, later on though I will have to perform some OS/Endpoint hardening as well as practice segmentation, logging, and firewall configuration.
