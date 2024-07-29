---
title: Historical Homelab Overview
author: diogo
description: An historical overview of my homelab
date: 2024-07-24 12:00:00 +0800
categories: [Homelab]
tags: [homelab, technology]
---
# Backstory

Around 2021 when I was interning as a software developer at a local company, I started to grow an interest in hosting my stuff, this came after hearing cool terms such as _auto deploy_, _auto testing_, _DevOps_, _plesk_, and _docker_. After building up some courage, I started to ask some questions to the technology manager at the time there and he began to teach me about the use of multiple domains and reverse proxies, admin panels, and he even showed me some parts of the configurations that they were using at the company, this talk had me sold the idea of running my servers and self-hosting my services.

# My first hardware
I managed to get hold of three old machines (for free!). I got two HP ProDesk 400 G1s, these were fairly poorly specked since they were only serving as POS terminals. These machines had an Intel Pentium G3220 paired with 2GB of RAM and 500GB HDDs. 
The third machine was the more powerful one, it was a Dell Optiplex 390 specked with an Intel i3-2120 with 4GB of RAM and 1TB of HDD storage.

# The first upgrades
Before I did any configuration or first deployment, I knew right away that I needed to upgrade these machines, so I started by ordering an 8GB stick of ram out of AliExpress (budget was tight and It somehow it still works to this day).
This 8GB stick ended up in the dell machine and I split the two 2GB sticks this machine had between the HP machines, therefore leaving the Dell with 8GB of ram and the HPs with 4GB each.
I also knew that I wanted a NAS, since I wasn't really queen on trusting my data on god knows how old hard drives, therefore, I got myself a Seagate IronWolf 2TB hdd to put my most important data on.

# The first deployment

After doing some research and more chatting to the previously mentioned technology manager I ended up going for Ubuntu Server as the operating system (OS) for these machines. 
Once I had the OS figured out I needed to set the rolls that these machines would play in this deployment, for that I needed a list of stuff that I wanted to run:
1. Nextcloud
2. Reverse-Proxy
3. Docker
4. Minecraft Server
5. Octoprint for remote 3D printer control
6. VPN

Since the Dell had the most powerful cpu and more RAM I chose it as the main server of my homelab, therefore, it was tasked with running NGINX as a reverse proxy, docker, minecraft server and VPN server. 
One of my HP machines got the role of being a NAS, therefore, it ran Nextcloud (and dependencies such as MySql and Redis) and some SMB shares so that I could access files more easily from my computers. 
The other HP machine had the singular task of running octoprint (and respective webcam stream for printer monitoring) since I didn't want any other task bottle-necking the weak pentium CPU.

This adventure was paired with a lot of trials and errors since this was my first time using linux for more serious purposes. After a couple of days, I had everything set up the way I wanted. It was an absolute blast, I was learning so much new stuff, and the feeling of learning new things by myself and making it work as intended was out of this world.
At this point, I still didn't know about the existence of tools such as [Nginx Proxy Manager](https://nginxproxymanager.com/) and was managing my subdomains directly via the config files, which wasn't that much fun.

This left me with the following setup:

![V1 of my homelab](/assets/img/posts/HomelabOverview/HomelabV1.png "Homelab Diagram V1")

# Re-doing everything

Part of the homelab experience is gaining more knowledge and finding better ways to do the same things. After a year of usage and adding more services to the mix, problems started to occur, drives filling up, managing the nginx config file and ssl certificates manually became increasingly more difficult to manage.
To solve the hard drive filling up I decided to use linuxe's Logical Volume Manager (LVM) which allowed me to combine multiple disks of different capacities and use them for a single big disk. In this case, I combined the space 500GB HDD with the 1TB one present in the Dell machine. 
In hindsight and due to the unknown past of my disks, I should have used a tool like [mergerfs](https://github.com/trapexit/mergerfs) which wouldn't leave me with a total loss of data if a single drive failed.    

The need to use LVM expand my disk space led me to just re-installing everything and doing some things better. During this major change, I also ditched the 'raw' use of nginx and certbot and started to use nginx proxy manager which made managing domains a breeze.

# Hosting my media                                                                                                                                                                                    
Early 2023 I stared to gain interest in hosting my own media, during this phase I tested out services like [plex](https://www.plex.tv/) and [jellyfin](https://jellyfin.org/). I ended up settling for jellyfin since I wanted to use as much Open-source software as possible, another feature of jellyfin that made it even more appealing was the ability to stream live television. 
To make live tv and recordings available on my jellyfin, I used [Tvheadend](https://tvheadend.org/) in combination with two DTV tuners. Contrary to IPTV and other digital services, most of the usb DTV tuners can only tune in to one channel at the time which makes it impossible to watch a chanel and record another ate the same time.
My dvd and Blu-ray collection was of a severe size. Therefore, I also found the need to increase my HDD capacity to suit these needs. Once again LVM came in handy since I managed to find an old 2TB CCTV HDD for cheap and it made the perfect candidate for expanding the storage of server.
The server tasked to handle all these tasks was the Dell optiplex, which struggled to keep up transcoding tasks and tv recordings. I must admit that I got superlucky with the use of LVM and a mix of disks with varying ages and varying working hours especially since I just added the previously mentioned CCTV HDD to my boot drive pool.    

# New server
After around two months of highly stressing my Dell optiplex and losing my mind to transcodes hanging and countless streaming errors due to the lack of adequate compute power, I decided that I needed an upgrade.
I started to browse the local used market (which is not very good in Portugal) and soon realized that if I wanted tons of computing power for cheap, I would have to go for an old enterprise server.
I ended up settling for a used dell r620. This old beast came with two Intel Xeons E5-2670, 64GB of DDR3 ram, two 900GB sas HDDs and dual redundant PSUs. Compared to my old machines this thing was a powerhouse but a new problem appeared. 
This server's chassis came configured with option to support 8 2,5" disks, but all my existing media is on 3.5" disks. To tackle this problem, I ended up moving all the drives to the optiplex machine and used that to server all my media as smb network shares.
To avoid having to set everything up again, I decided to clone the drive from optiplex server to an SSD and use that as a boot disk to my new r620 server.
        
This left me with the following setup: 
![V2 of my homelab](/assets/img/posts/HomelabOverview/HomelabV2.png "Homelab Diagram V2")

# Virtualization

More recently, I embarked on the journey of using Proxmox, and I'm gradually migrating all my services and applications to LXC containers. 
This shift has introduced new efficiencies, reduced operational costs and added capabilities to my homelab setup. 
In a future post, I'll delve deeper into this transition, exploring the benefits, challenges, and detailed steps involved in moving to Proxmox and LXC containers.

# Conclusion
In conclusion, the journey of building and evolving my homelab has been an incredible learning experience, marked by trials, errors, and many upgrades. 
Starting from basic setups with old, underpowered hardware, and gradually expanding my systems to meet my needs.

Thanks for reading and hope to find you around. Feel free to contact me through my social media if you want to discuss any of the previous topics.


