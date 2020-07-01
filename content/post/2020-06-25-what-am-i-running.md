---
title: 'What Am I Running?'
date: 2020-06-25
thumbnailImagePosition: left
thumbnailImage: /img/posts/2020/06/what-am-i-running/header.jpg
coverImage: /img/posts/2020/06/what-am-i-running/header.jpg
coversize: partial
coverMeta: out
metaAlignment: center
categories:
- Homelab
- Virtual Machines
- Docker
tags:
- homelab
- vmware
- esxi
- unraid
- docker
showTags: false
showPagination: true
showSocial: true
showDate: true
comments: false
summary: "Between the two servers I have on my network, I run a lot of stuff. Here's the gist of what it all does."
---
{{< alert info >}}
**This list is current at the time of writing, but it tends to change often!**
{{< /alert >}}

{{< wide-image src="/img/posts/2020/06/what-am-i-running/diagram.png" title="The relevant bits of the network diagram" >}}

The two servers that aren't the tiny box I have running pfSense, are running mostly for VMs, and storage, respectively.

## titanium
The main server I have running is a dell R710. Unfortunately, this is a 2.5" SFF model, but I don't need a ton of storage here anyway. This thing is running 8 600GB drives in RAID 5 for a total of 4.2TB of usable space. It's rocking dual X5660 Xeons, and 120GB of 1333 MHz DDR3 RAM, which is plenty for what I need it to do.

Currently, this machine is running ESXi 6.5U3. Technically, I could be running 6.7 with the 5600 series Xeons, and I very well might do that soon, though as of right now, it's on 6.5 for the compatibility with Dell's OMSA, for things like mainly live RAID tinkering without having to take the thing down and boot into the RAID card BIOS to kick off and wait for a rebuild should something fail.

I have everything ordered by IP address here, so let's roll with that.

### boron
This machine is an Ansible controller. It actually doesn't do anything other than run Ansible at the moment, and I use it to sort of streamline my workflow with these VMs. As of right now, I only have two playbooks, though as I get more confortable, this will change in the future. Right now, I have a playbook that helps set up new VMs, by copying the config and templating it to configure it to work with my APC UPS that's in the rack, as well as check for updates and install some basic packages.

The other template I have just is a simultaneous `apt update && apt upgrade -y` on all of my Debian-based machines. I haven't yet configured it to be able to control Windows or anything, though I hope to soon! For those interested, the stuff I have working is avaiable on [GitHub](https://github.com/TechGeek01/Homelab-Ansible) for you to check out!

### dc01
Right now, this thing doesn't do anything. I set it up a while back with the intent to learn and configure some AD stuff like authentication and DNS and such, and it never happened, so there it sits, doing nothing, and not running.

And for those of you wondering, I get a certain number of licenses of Windows Server and VMware Workstation and such free through my schooling, since I'm an IT student, which is pretty sweet if I say so myself.

### OMSAwin
Sort of the same as before. It doesn't do much. Right now, this is running Server Core, just cause I wanted something lighter weight than full blown Windows Server. This thing exists solely to host the Dell OMSA client, because for whatever reason, I can't connect directly to the VIB installed on ESXi, I have to use Windows Server (because it doesn't work on Linux, only Windows) to install the web client, that actually connects to the VIB, pulls data, and creates the web dashboard I can sign into and manage things.

### oxygen
Right now, this is my favorite server, just cause it hosts a bunch of cool stuff. At the moment, everything's inside of Docker, so let's run through it all!

#### Reverse proxy
This thing runs `nginx` for the purpose of having a reverse proxy, because having a bunch of subdomains on the same port that I can reference instead of remembering DNS names and port numbers is dope.

#### External reverse proxy
The same thing as above, but that hosts all of my public services, so that I can port forward to that, without exposing things I want to be LAN only.

#### Heimdall
I have a lot of stuff going on here, so I'm using Heimdall for a dashboard for it.

#### Pihole
Ads are dumb. Plus, why download ads and block em in a browser when I can block em everywhere on the network at once? Except apparently YouTube, cause those clever guys made the ads come from the same IPs as the videos, so I can't block ads with DNS without also blocking the videos.

#### Watchtower
Docker involves a lot of updates. Sometimes, I forget to update things, so I have watchtower handle that for me. I have it check once a day for updates on the containers at like 4AM.

#### Key database
I get keys for things like Windows, Windows Server, and such through school. Between keys I own already, those keys, and from old computers customers use for data transfer onto a new one when it's time for an upgrade and want to recycle, I have a lot of these keys. Most of them are Windows 7, but I digress. Anyway, I wanted a place to store them that was better than a text file somewhere, so I wrote a custom dashboard for it.

{{< alert info >}}
Because this uses both PHP, and a database to store stuff, I need `php:fpm` and `mariadb` on top of the Nginx container to do this
{{< /alert >}}

{{< wide-image src="/img/posts/2020/06/what-am-i-running/key-database.png" title="The key dashboard in all its glory" >}}

I have a bit of work to do to tidy up the backend code and make it a bit more streamlined when I'm entering or editing keys, but it works for now.

#### DMZ web server
This is a web server for serving up my original idea of a blog or something that was publicly accessible on my network. Given that I have satellite right now, that was put off, and I ended up finding this solution with GitHub. I may reuse this Docker-compose cluster at a later date for something else though.

{{< alert info >}}
Again, I needed `php:fpm` on top of the Nginx container
{{< /alert >}}

#### Syslog dashboard
This is where things get a bit interesting. I needed/wanted a syslog server, and a central dashboard. The way this works is I have a 50GB virtual disk for logs, and it's NTFS formatted, and mounted as a Samba share, so that I can browse it should I need/want to. Again, like the key database, this is a custom dashboard.

Where things get interesting though, is that I didn't want to handle checking it by hand. The `php:fpm` container is built with a Dockerfile, and I've tacked cron and mail stuff on top of it, so that I can have it check the log directory every 5 minutes. If I'm over 90% used in that 50GB disk, it deletes oldest logs first until it's under 70%, and then sends me an email summarizing what's been deleted, and the amount of space that's been freed up.

For some reason, I couldn't get syslog-ng working inside the PHP container when I built it, so it's a separate container.

### fluorine
This one's another one of my favorites. This guy's running Mailcow, and using SendGrid as an SMTP relay. This isn't public mail, and this isn't for personal use. It's using my internal domain, which is different from this one, and is used mostly for custom addresses for things like Unraid to send me SMTP notifications. All this because **helium@mydomain.com** looks a whole lot better than an arbitrary Gmail address I had to make, and this way, I can have different ones for everything instead of having to hope a Gmail is up for grabs, or reusing the same one for everything.

### copper
As of right now, this is just Grafana, and underlying stuff like InfluxDB for monitoring and dashboard-y things. I still have a lot to learn, and I need to get around to actually foguring out how to make a dashboard in Grafana that shows what I want.

### Kali
This VM is mostly off all the time, though when I need to test something, or a buddy of mine is like "hey, I got this new thing. Can you try and pentest me so I can see what the security's like?", I fire this guy up for a bit.

### carbon
This server was using `apt-mirror` and used to host a local mirror of all of the `apt` stuff, since I used to have way more VMs, and it was easier to pull once overnight, and reference a local repo than to download 10 copies of the same sets of updates on satellite internet.

I may tear this down at some point. It's not used with anything, though it's still able to be connected to if I decide I want to.

### CentOS PXE
This was a VM I set up a long time ago following a tutorial by someone who I've forgotten who wrote it, but it hosted ESXi 6.5 and 6.7 over PXE, so I could fire this thing up, and PXE boot a server and kick into an installer without having to burn a USB.

### FOG PXE
This is meant to be replacement of the CentOS server, though I never really ended up fully setting it up. FOG works and all, but I never got around to working a bunch of ISOs into the menu. Eventually, I want to replace the CentOS server, though I'm in this really weird state, where I don't really use either server, and so it's not that pressing of a matter to look into. Like they say, there's nothing more permanent than a temporary solution.

### RIPE probe
Self explanatory. Since RIPE NCC rolled out their software installations, I have one of these running. Lets me give them metrics like throughput on the network and rough location of my IP and such, in exchange to be able to query their database against all of these stats.

## helium
This might be my more favorite of the two servers, just cause it's packed with storage. Not as much as it could be, but I still love it. It's at 22TB usable at the moment, though I'm getting my replacement drive from an RMA'd 10TB drive that was DOA in a couple of days, so I should be able to kick this up to 30TB pretty soon.

Network shares on this include backups, Plex stuff, important documents, one for Funkwhale media, some download stuff, and a junkyard share that's a shared effort between myself and a friend of mine. (long story short, we got sick of sharing things on Facebook Messenger, and their 25MB limit and not liking some files like .exe, so we linked our networks, and that junkyard share is accessible by both of us.)

{{< alert info >}}
This site to site link between my friend and I, and what started all of this, is going to get its own post very soon, where I'll be able to go into more detail!
{{< /alert >}}

I'm not running much for VMs on here. Only one Windows Server VM that hosts Veeam, so I can back up my VMs from ESXi on a nightly basis. As for Docker, there's a few things running on this guy, since Unraid supports Docker out of the box, and auto checks for updates.

### Plex / Jellyfin
I use Plex primarily, but I installed Jellyfin once just to check it out. It's growing on me, but I still prefer Plex.

### Folding@home
This doesn't get much use these days. Now that it's summer, it gets pretty toasty in the room when I run this thing, but over the winter, I had it going full blast for a while.

### Minecraft server
This one was spun up as a test to see how easy it was to do, and I plan on eventually hosting a small server for some friends, though being on satellite, it's not really an option at the moment.

### Bitwarden
I eventually plan on moving my Bitwarden stuff from their servers to a self hosted version with `bitwardenrs`, but since I'm on satellite, again, this hasn't completely happened yet.

### Deluge VPN and related
The primary container here in this bit is binhex's DelugeVPN, where it's running through a VPN as an always-on client. Other containers like Sonarr and Radarr proxy through this container.

## So what's next?
Frankly, I don't even completely know. I have so many things that float onto and off of my homelab to do list that it's a large list at this point. The immediate next steps are probably to get more comfortable with Ansible and Grafana, but who knows where I'll end up after that!