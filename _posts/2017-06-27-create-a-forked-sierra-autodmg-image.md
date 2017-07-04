---
layout: post
title: "Create a forked Sierra 10.12.5 (16F2073) AutoDMG image"
author: "Geoff"
---

New year, new Macs... now your stuff's broken. And since many of us need to have working Netboots available to get these new Macs out -- and since we can't afford to wait until we get our hands on 10.12.6 -- we need a way to make deployable forked 10.12.5 disk image. But there's some good news: though most definitely not on purpose, Apple made the process bit easier this time.

### What you'll need

1. AutoDMG (if, for some strange reason, you don't have it already) [<a href="https://github.com/MagerValp/AutoDMG/releases" target="_blank">link</a>]
2. A macOS Sierra Installer [<a href="macappstore://itunes.apple.com/us/app/macos-sierra/id1127487414?mt=12" target="_blank">link</a>]
3. macOS Sierra 10.12.5 Update for 2017 iMacs [<a href="https://support.apple.com/kb/DL1921" target="_blank">link</a>]

### What you'll need to do

Once everything's been downloaded, launch AutoDMG, then drag and drop the macOS Sierra installer you grabbed from the Mac App Store into the app. It'll pre-populate with some updates (e.g., iTunes and Apple Remote Desktop) and, depending on your use case, either download and check to apply the updates or skip it. 

Now, before building, you'll want to open the *macOS Sierra 10.12.5 Update for 2017 iMacs* dmg you downloaded, and drag and drop the enclosed pkg into the *Additional software* section in AutoDMG.

Then... **Build**.

Once it finishes, you'll have a working 16F2073 build of 10.12.5 to hold you over until 10.12.6 drops.
