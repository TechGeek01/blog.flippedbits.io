---
title: "How Do I Segment My Network?"
date: 2020-07-01
thumbnailImage: /img/posts/2020/07/how-do-i-segment-my-network/header.jpg
coverImage: /img/posts/2020/07/how-do-i-segment-my-network/header.jpg
coverSize: partial
coverMeta: out
metaAlignment: center
categories:
- Networking
tags:
- network
- firewall
- pfsense
showTags: false
showPagination: true
showSocial: true
showDate: true
comments: false
summary: "A lot of people ask me why I have so many VLANs, and how I split everything up on my home network, so let's walk through it!"
---
I have a lot of different stuff that I'm dealing with on my home network, and I like to keep it segmented. There's no reason to dump everything on one VLAN when I can isolate certain things.

I have a lot of VLANs, and all but management are **/24**s. This allows me to encapsulate all of the **/24**s with the **/16**. This means that for, say, my ESXi server at `10.0.10.10`, the iDRAC on that server for out of band management is `10.99.10.10`.

{{< alert info >}}
**Admins** is an alias in the pfSense firewall, that contains my main computer, my laptop, and my phone. This lets me access things like management without allowing the whole VLAN access.
{{< /alert >}}

## VLAN 10, {{< hl-text yellow >}}Servers{{< /hl-text >}}
For server-related things, such as ESXi and most VMs.
* Can access {{< hl-text purple >}}storage{{< /hl-text >}}, and {{< hl-text green >}}media{{< /hl-text >}}.
* The ansible controller can access **DMZ** and {{< hl-text orange >}}IoT{{< /hl-text >}}.

## VLAN 20, {{< hl-text purple >}}Storage{{< /hl-text >}}
Sort of similar, but for storage devices. I have things like Unraid sitting here.
* Can access {{< hl-text yellow >}}Servers{{< /hl-text >}}, and {{< hl-text primary >}}end devices{{< /hl-text >}}.

## VLAN 30, {{< hl-text green >}}Media{{< /hl-text >}}
This VLAN houses things like Plex and Funkwhale.
* Can access {{< hl-text yellow >}}servers{{< /hl-text >}}, and {{< hl-text purple >}}storage{{< /hl-text >}}.

## VLAN 70, Security
Eventually, when I get around to installing and setting up some cameras, they'll go here.

{{< alert info >}}
**Security** is isolated
{{< /alert >}}

## VLAN 80, DMZ
Anything I want public-facing goes here.

{{< alert info >}}
**DMZ** is isolated
{{< /alert >}}

## VLAN 99, {{< hl-text red >}}Management{{< /hl-text >}}
Houses management interfaces for switches, out of band management for servers, access points, and the like.
* Can access the **syslog server** on **601**, and **UDP 514**.

## VLAN 100, {{< hl-text primary >}}End devices{{< /hl-text >}}
My main user-facing devices, like computers, laptops, and such go here.
* Can access {{< hl-text yellow >}}servers{{< /hl-text >}}, {{< hl-text purple >}}storage{{< /hl-text >}}, {{< hl-text green >}}media{{< /hl-text >}}, and {{< hl-text orange >}}IoT{{< /hl-text >}} (for Chromecasts and such).
* **Admins** can access **security**, **DMZ**, and {{< hl-text red >}}management{{< /hl-text >}}.

## VLAN 101, {{< hl-text orange >}}IoT{{< /hl-text >}}
Things like Google Home, or Alexa devices.
* Can access {{< hl-text green >}}media{{< /hl-text >}}, and {{< hl-text primary >}}end devices{{< /hl-text >}}.

## VLAN 199, Guest
Guests that want internet have their own isolated VLAN so they can't see anything else.

{{< alert info >}}
**Guest** is isolated
{{< /alert >}}
{{< alert info >}}
**Guest** has limiters, since I'm on a 20/5 connection
{{< /alert >}}