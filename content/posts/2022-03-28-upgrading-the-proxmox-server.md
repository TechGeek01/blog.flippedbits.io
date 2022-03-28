---
title: "Upgrading the Proxmox Server"
date: 2022-03-28
thumbnailImage: /img/posts/2022/03/upgrading-the-proxmox-server/header.png
coverImage: /img/posts/2022/03/upgrading-the-proxmox-server/header.png
coverSize: partial
coverMeta: out
metaAlignment: center
categories:
- Networking
- Servers
- Upgrades
tags:
- network
- supermicro
- server
- proxmox
showTags: false
showPagination: true
showSocial: true
showDate: true
comments: false
summary: "I got sick of running Proxmox on an old, power-hungry server on spinning hard drives, so I did something about it."
---

Until recently, Proxmox has been dutifully running on some old Dell servers. Primarily, it ran on an R510, though I did also have an R710 that sometimes got used as part of a cluster for some things. All in all, these aren't very powerful or power-efficient machines these days.

On the R510, Proxmox was running with dual Xeon E5620's, and 96GB of RAM. I had 4 drives in it, 2x3TB and 2x2TB, in a ZFS mirror with 5TB usable. Unfortunately, while this was faster than single hard drives, it was still slow, and was bottlenecked by older, slower processors as well. On top of it all, the whole thing idled at about 250W. The R710, while it was rarely used, was much of the same story, with dual X5660's, and 64GB of RAM with 8x600GB in RAIDz1 for 4.2TB usable.

I decided it was time for something new, and set out to build a replacement server. My goal was to make it both faster and more power efficient, while hopefully still upgrading the storage, and I think I achieved that goal.

## The specs
The chassis here is a Supermicro 216. While I could have gone with other, slightly older chassis, I wanted something fast and modern, so a SAS3 backplane 216 seemed like a decent choice considering the deal I got on it.

I decided to go with the same Supermicro X10 platform as the Unraid server, as it went well for that upgrade, and didn't break the bank as much as X11 did. This new server is running dual E5-2630 v4's, the same as the Unraid server. I could have gone way more powerful for this server, but anything higher than a 2630 in v4 costs way more, and I don't use a whole ton of processing power at any given time anyway, so I figured I could upgrade down the line if I ever needed it. These things are 10 core, 20 threads each, which is a lot of room to play with.

I kitted it out with the same 16GB sticks of ECC DDR4 that I put in the Unraid server, though it was a bit more expensive this time around, so I ended up only buying 6 of them instead of 8. I did, however, prefer the performance of quad channel, and the headroom of 128GB over 96, so I elected to take 2 of the sticks from Unraid and put them in here.

Now, for storage, this is where we get to the fun part. This 216 chassis has a `BPN-SAS3-216EL1` backplane, which lets me put some pretty balls to the wall storage in here. I threw 4 SSDs in it, specifically, the Toshiba PX05SRQ192, which are 1.92TB SAS3 SSDs, capable of 1800MB/s reads and 850MB/s writes. All 4 of these are backed with a Dell H330 flashed to IT mode. This card is SAS3008-based, and lets me run all 4 of these SSDs at pretty fast speeds. Combine this with RAIDz1, and you have 5.76TB of usable storage, one drive redundancy, and a bunch of VMs where the Proxmox BIOS screen and GRUB menu (where there's intentional timeouts to wait for you to press a key), take longer to go by than the entire rest of the boot process, and most of that is waiting for the network link to come up.

## The result
{{< image src="/img/posts/2022/03/upgrading-the-proxmox-server/server-racked.jpg" title="All in all, this went pretty well, other than that the SAS cables are way longer than I needed them to be." >}}

I set some pretty ambitious goals here, as I wanted something that was faster, better, had more performance in general, and still wanted it to suck less power. So how did we do? Well, this is definitely more modern hardware, and has a less ewaste-y vibe to it than an R510.

{{< image src="/img/posts/2022/03/upgrading-the-proxmox-server/cpu-comparison.png" >}}

I think the processing power increase speaks for itself, though it definitely isn't the most powerful processor in the realm of v4 Xeons.

However, there's one more thing I wanted to address: Power. Previously, the R510 used anywhere between 250 and 270W to do its thing, and that's without the processor being hit too hard. This new server boots in about a fifth of the time VM booting is near instant, and everything just feels a whole lot better. And the whole thing only sucks back 130W.

The next project on my list is upgrading pfSense, since I want a server that gets enough airflow to PCIe cards to not kill an SFP+ NIC. I've been eyeballing the Supermicro X10SLH-N6-ST031 because it seems perfect for a router, and it's such a stupid looking motherboard. Maybe one day, I'll actually bite the bullet and buy one...