# Acquire Hardware

This project originated from my desire to build a homelab, as well as my senior capstone project, in which I'll perform a full security assessment of the open-source LMS platform, Chamilo. I figured, why not kill two birds with one stone? I could use a single machine to both host the LMS I'm going to assess and self-host services and projects, such as VPNs, file servers, a DHCP server, a web server, and whatever else I may want to experiment with.

Originally, I had planned to gather and buy parts through various marketplaces, stores, and online retailers. Unfortunately, AI has beaten me to it. Prices for many basic components have become inflated, putting them well beyond my $500 budget. Of course, I had the option to endure the inflated prices and buy the parts I needed, but I'd rather wait until supply catches up and I can buy something at a reasonable price.

Regardless, I still needed a system to tinker with, and I'd rather not use my main machine and risk messing with something I wouldn't know how to fix. Instead, I went with Plan B. In my closet, I had an entirely forgotten late-2015 iMac all-in-one desktop.

After powering her on for the first time in years, I immediately remembered why she had been abandoned. The system was SLOW! So much so that each reboot took about five minutes just to reach the login prompt. I decided to inspect the machine to see if it could even serve as a suitable homelab server.

The specs are as follows:

- 3.1 GHz Quad-Core Intel Core i5
- 8 GB 1867 MHz DDR3 RAM
- Intel Iris Pro Graphics 6200 (1536 MB)
- 1 TB HDD

Despite the operating system consuming about 3/4ths of the RAM while idle, the root of the system's performance problem was the HDD.

There are various solutions to this HDD issue, as it's fairly common among iMacs from 2012–2019. I could open up the desktop by removing the adhesive from the 21-inch screen, remove the HDD, install a newer SATA SSD, then reapply the adhesive and pray the display doesn't fall off later on.

But compared to the lazy solution, where I use the desktop's USB 3.0 interface with a SATA-to-USB adapter, I'd achieve essentially the same result with substantially less risk. So, I ordered a Patriot 256 GB SATA SSD and a Sabrent SSD enclosure with a USB adapter. After formatting the new drive and placing it into the enclosure, I prepared to use it as the installation destination.

To address the operating system issue, I decided to install Debian 13 with XFCE. Apart from a few classes that used very basic Linux CLI commands, I wasn't particularly comfortable with Linux. XFCE gives me a fallback GUI in case things go wrong or I encounter an issue I don't know how to fix from the CLI.

I used Rufus, a bootable USB utility, on my laptop to prepare a USB drive for the installation. I tested a Linux image and confirmed that the hardware was perfectly capable of running Linux.

Now, this is where the first roadblock appeared. After plugging in everything I needed, I booted into the installation screen, and not even a minute into the process, I ran into a wall of fatal disk I/O errors.

Clearly, there was some sort of issue with the SSD installation. The question was, what?

After waiting a few minutes for the hung OS installation process to figure itself out, I caved and forcibly powered down the machine. I performed some basic troubleshooting. Were the USB connections properly seated on both ends? Was the SSD properly seated? Was the drive recognized by my laptop and main machine?

After verifying that everything was functional, I figured perhaps the USB port was faulty. It was an old machine, after all.

I plugged the adapter into the USB port next to the original and repeated the entire boot sequence. And, just like a charm, I saw the installation manager successfully proceed.

All I had to do next was choose the basic installation options, and soon enough, I booted into the login screen. From there, I researched and ran a couple of health checks on the SSD, all of which returned healthy results.

Next was connecting the server to the network.

As a teenager, I bought this server-grade Cat 8 Ethernet cable capable of 40 Gbps throughput. Now, looking back as a senior Computer Science major, this was incredibly stupid. The connection is heavily bottlenecked by my Verizon Fios G1100 router, which has a maximum Ethernet port throughput of 1,000 Mbps, only 1/40th of the cable's rated capacity!

Granted, this was purchased back when technology was relatively cheap, so I didn't pay a particularly large sum for it. Although, if I ever wanted to start a mini data center in the future, I'd already have a cable capable of supporting much faster connectivity (assuming I upgraded the rest of the network infrastructure).

I bought a NETGEAR GS308E managed switch, which allows me to configure VLANs. I also bought two Cat 5e cables to connect the homelab and my main machine to the switch. I then used the Cat 8 cable to connect the switch to the router.

I tested connectivity by pinging a couple of sites, and everything worked flawlessly.

I'm not entirely sure why I didn't do this sooner, considering it only took a couple of hours and wasn't exactly difficult.

Next was to begin applying the concepts I've learned while studying for my CompTIA Security+ exam.

This should be very fun!
