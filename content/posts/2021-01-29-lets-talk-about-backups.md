---
title: "Let's Talk About Backups"
date: 2021-01-29
thumbnailImage: /img/posts/2021/01/lets-talk-about-backups/header.png
coverImage: /img/posts/2021/01/lets-talk-about-backups/header.png
coverSize: partial
coverMeta: out
metaAlignment: center
categories:
- Data
- Backups
tags:
- data
- backups
- backblaze
- hard-drives
- tape-drives
- bluray
- dvds
showTags: false
showPagination: true
showSocial: true
showDate: true
comments: false
summary: "I see a lot of discussion on backups, and a lot of people that don't do it right, or don't do it at all. Let's talk about that."
---

Lately, I've seen a lot of talk about backing up your data. And that's fine. The problem is that when we talk about backups, most people either don't take backups at all, or if they do, they don't know how to do it properly. I may not be an expert on this, by any means. However, I do take frequent backups, and I have 100% confidence that they'll work when I need them, so let's talk about it.

The largest problem I see by far is that people don't take backups at all, or they don't do it properly. I work tech support, and see a lot of people that when we do data transfers or backups for people, that we can't recover something. In these cases, I tend to ask a lot of people if they back up their stuff. And the scary reality of it is that 9 out of 10 people tell me they don't.

Let me preface what I'm about to say with "I get it." I used to be this person. I used to not take proper backups, and most of this didn't matter all that much. At some point, I lost data on a 10 year old external hard drive, and replacing that data meant redoing a lot of school work. I've been burned by data loss in the past, and my mindset is now set to "never again," and while the backups I take might be overkill for most people, it's a proper system that I know I can rely on if I need to get any of it back.

> Oh, I don't need backups. There's nothing important on here at all.

I don't buy that. Everyone has stuff that's important to them, whether they know it or not. If it's "just some work documents" or "just some pictures I took a few years ago on vacation" then it may not be the end of the world. But the fact of the matter is that if you lose these things, they're either going to be an absolute pain, and require a lot of work to replace, or they're not replaceable at all. If you have data that has any sort of value to you, it should be backed up.

The rule that I tell people when I explain this is that if you think you have enough backups, make another one. Because the people that don't think they need a backup, they probably do. The proper rule that should be adhered to is the 3-2-1 rule:

* **{{< hl-text green >}}3 copies{{< /hl-text >}}** of anything you deem important
* On at least **{{< hl-text green >}}2 different forms of media{{< /hl-text >}}**
* And **{{< hl-text green >}}one of those{{< /hl-text >}}** should be off-site.

If there's anything I've learned, it's that if data you care about doesn't exist in at least 3 places, it doesn't exist at all.

> Yeah, I take backups. I have an external hard drive I use.

Of the small subset of people I ask that actually take backups, most of the time, something like this is the case. If you're using an external drive to back up your data, that's great. What's worse is when it's a 10 year old drive, and they don't remember where they got it from. And their backups are manually copying some files over every 6 months when they plug it in.

And herein lies the problem. Most people that "take backups" of their data, aren't doing it properly. When I encounter someone that says they take backups of their data, the question I pose to them is "have you ever deleted your data to test if your backups work?" And almost every single time I ask this, they look at me like I'm completely insane. Of course, I don't mean you should delete your data to test backups. You can test them without deleting your data. But if you're going to use your backups, it's in a situation where the original data is unreliable, so that's the situation you're going to be in. If the idea of restoring from your backups scares you, then your backups don't mean anything.

# So how do I properly back up my data?
To explain how we should be properly backing things up, let's first define some rules, and go over some misconceptions.

A proper backup should protect you against data loss. And by data loss I mean any data loss. Whether that's drive failure, accidentally deleting something, or some other disaster, a proper backup should protect against that.

## Back up frequently, and reliably
The major issue with how most people take backups, is that it's either inconsistent, infrequent, or both. To a lot of people, proper backups are "too much of a hassle" so they just... don't do that. Let's jump back to the example with the external hard drive I gave above.

There's two problems here. First of all, the 10 year old, old hard drive isn't as reliable as something new. If it's a drive that they don't remember not having it, or perhaps it makes a weird clicking sound when it's plugged in, it's probably not the most reliable medium for taking backups on. In a case like this, that external drive is probably more likely to die or have problems than the drive in the computer they're backing up.

Secondly, a lot of people tend to only back things up once or twice a year. If you have a 6 month old backup, and you accidentally delete something, you need to restore from a backup. But if it's an important document that you reference or revise frequently, that backup you made isn't going to be terribly effective if you've changed that file a couple dozen times since you last backed it up 6 months ago.

## Separate your backups
So you're backing up your documents once a week now. Great! But your backup is to copy your files from your desktop into some other folder on your computer. Well, you're halfway there. Sure, if you accidentally delete something, that protects you there. But remember, hard drives are mechanical, and they fail. Your backup is no good if it resides on the same drive you're backing up, because if that drive dies, you lost your backup too.

## Remember that RAID is not a backup
Most people, when they think of backups, think of drive failures. And believe me, I've seen my fair share of people that have had drives fail, or become unreliable because they're about to fail. The first thing people think of here is that if you have two drives, this protects against drive failure.

In this case, there are two options. You can either back up your data onto a second drive, so you have a backup in case something fails. Or, you can mirror the drives to add redundancy, so that you don't have to actively back anything up, and your data exists on two drives. In this case, the latter is an appealing option for most people, because it mitigates having to make backups regularly. Unfortunately, what most people don't understand is that RAID is not a backup. RAID is uptime.

RAID comes in many forms, but at its core, it's meant to provide speed (in some cases, at an increased risk of data loss), or in our case, redundancy, so that it's harder to lose your data. It does this by sacrificing in some way, part of the capacity to add more protection. If you have one drive of redundancy in a RAID array, you can lose one drive and still have your data. It's only when you lose two drives that it becomes a problem.

> Well, that certainly sounds an awful lot like taking backups, doesn't it?

Well yeah, it does. But they're not the same thing. RAID protects you against data loss *due to drive failure*, but losing your data due to hardware failure is a very small subset of "losing your data." If a drive in a RAID array fails, your data is fine\*. If, however, you accidentally delete a file, RAID will not protect you. Fundamentally, that array functions as a larger virtual drive, and just like a single drive, will not protect you from deletion, corruption, or the like.

> RAID will happily replicate all your changes to the whole array. Even the ones you don't want it to.

{{< alert info >}}
\* Technically, hard drives occasionally have read errors, where a chuck of data on the drive can't properly be read. While your data as a whole doesn't go away if RAID protects you with redundancy, if you lose a drive, and have no other protection, there's still the chance you can hit a read error trying to read the data in a file, in which case, part of that file may become corrupted. An intact RAID array protects against this, but a degraded one has the same risk of it happening as a single drive does.
{{< /alert >}}

## Backup to a separate machine
The easy way to make backups is to back your data up to a separate drive, either internal to the computer you're backing up, or to an external drive. This is actually mostly okay, but if you get hit with ransomware, those backups are going down with the rest of your computer.

The best approach to mitigate this is to either back up to an external drive that's not always connected (that is, you disconnect it when you're not backing up) so that it can't be affected it ransomware hits, or to back up to a separate machine entirely. A NAS, or network attached storage, is a separate computer of sorts whose sole purpose is to manage storage, and serve up a network share for you to access. This share can be a single drive, or a pool of several drives in the form of a RAID array like we discussed before.

The basic premise is that you populate a NAS with hard drives, and use the software on it to configure the drives and the storage array. Then, you hook it up to your existing home network, and those drives become accessible over the network to back up to.

## Don't stick everything on the same type of device
Let's face it: The main reason for backups, aside from user error, is hardware failure. Most of the computers we use and store data on store data on mechanical hard drives, because they're cheap to make, and can pack a lot of data in them for not a lot of money. Mechanical drives fail. There's a lot of moving parts, and sooner or later, they can and will fail. Sometimes silently, and sometimes spectacularly, but it happens.

Don't put all your eggs in one basket. If all your data is stored and backed up on hard drives, that's fine short term, but it leaves you at higher risk, as the same problem that can kill your computer and make you rely on the backups you've taken, can also affect the backups in the same way.

Diversify your data. Whether that's hard drives, cloud backups, SSDs, DVDs, Bluray, tape, whatever. Keep something on something different so that one problem can't kill all your copies of your important cat photos.

## Keep something off-site
Okay, I know what you're thinking:

{{< alert info no-icon >}}
You've already covered proper backups to a separate machine, and different media types. Isn't my data perfectly safe there?
{{< /alert >}}

Well, for the most part, yes. But what if your house burns down? Or it gets broken into, and all your computers and drives are stolen? When that happens, you want something else to make sure you can recover from that.

It doesn't matter if this is online, or carting external drives somewhere else. Just make sure something's off-site in the case of a disaster. If your house burns down, you have much larger immediate problems to worry about, but you still don't want to lose all those precious vacation photos that you can't replace.

# How do I back up my data?
By now, you know what you should and shouldn't do when backing stuff up. But what do I do to back up my important stuff? I've been burned by data loss before, and never again, so my backups are more thorough than most people will be doing. That being said, I have a few things to back up here.

* **Main desktop:** My main computer I use on a daily basis.
* **Upstairs computer:** The other desktop in my house. Rarely gets used, but it's still in use sometimes.
* **School external drive:** This is an SSD in an enclosure I use for school. I'm an IT student, so this is basically essential for school, and holds all of my classwork.
* **Files on my NAS:** This NAS holds all of my stuff that's not on my computers.

The NAS here is my primary backup destination for most of the stuff, so to understand these backups, let me explain how this NAS is set up. This NAS is super overkill, and holds potentially more drives than I could ever need. It's actually a pretty awesome custom build, that I [wrote about previously]({{< relref "2020-12-02-upgrading-the-unraid-server.md" >}}).

Most of this NAS is for things like movies and TV I've ripped from DVDs and Bluray, and it's used for Plex. The important part for backups are two shares. The "documents" share contains important documents, like old photos, or scanned documents, invoices, and the like. The "backups" share is the destination for all of my backups from elsewhere around the house.

## Macrium Reflect
Macrium Reflect is the core of my backup strategy here, and is used on both desktops for backing up a lot of stuff.

* The main desktop has the boot drive with Windows imaged nightly, and backups are retained for 3 months.
* Files on this desktop aren't all on one drive. I have a second hard drive in it, so the important files are backed up as a file backup nightly as well, retaining a month of backups.
* My school drive is connected to the main desktop as well, since that's where I work on stuff when I'm not at school. That's backed up nightly on week nights, retaining 20 weeks of backups. This ensures that not only can I get my data back, but I can roll back to any point within a 16 week semester, should I need to.
* The upstairs desktop doesn't have any important stuff I can't afford to lose, and is mostly just for internet use. There's only one drive in it, and it's backed up twice a week, retaining two months of backups.

## Backblaze
I use Backblaze for cloud backup, and it's a nice peace of mind to have. Currently, I only have it on the main desktop, as I can't use it with the NAS (at least not on their $6/month flat rate tier), and there's nothing too important on the upstairs computer.

Backblaze continuously backs the same documents and files that I grab with the Reflect file backup, and stores it on their servers.

## Off-site stuff
Twice a month, I visit my parents. As an extra precaution, and to also back up some of the documents on the NAS, both the documents and backups shares are backed up to a couple sets of spare hard drives.

They don't all fit on one drive, so each of the two sets is a few drives, and everything is split and crammed on the drives to fit both shares. This is in the form of a manual copy over the network to the drives, and then each file is verified once it's copied so that I know the backup isn't corrupted.

All drives in these backup sets are encrypted with VeraCrypt to make sure that no one can get to my data if they fall in the wrong hands, and these two sets are backed up and swapped out roughly every two weeks when I visit my parents.

# In Summary
Is this backup solution I have super overkill? Probably. Most people probably don't need to be as paranoid about this as I am. But, this ensures that if I lose anything, I can 100% know I can restore from a backup that's less than 24 hours old.

I don't expect everyone to heed my advice and adopt my backup strategy, but I hope you at least learn something, and are able to benefit from at least some of the information provided. I see a lot of customers have us try and pull data off of some old laptop or something. And it's dinged on a corner or two and has clearly been dropped at least once. In cases like that, you have two options. Take overkill, proper backups, and not lose any data when that drive dies from dropping it, or pay 4 figures to a professional for data recovery. And just like that, suddenly taking proper backups doesn't seem like as much of a hassle to the end user as they originally thought, because no one wants to pay that much money to get data back that should have been backed up in the first place.