---
title: "How I back up my small servers"
date: 2020-12-24T22:35:17+01:00
draft: false
categories: ["blog"]
tags: ["sbc", "raspberrypi", "homelab"]
---

We all have heard about horror stories concerning SD card reliability when (mis)used as root disks on SBCs; With such a track record a good backup strategy should always be in place to prevent data loss and shorten downtime when a failure occurs. Fortunately I only had one MicroSD card casualty over the years, and the following backup method did its job right to recover from it.

I like to store backups on the local network because it is faster, easier to access and safer (I don't encrypt my backups). As a backup server I rely on an ancient NAS I decommissioned a few yars ago because it was too slow for media storage (we're talking single-core SPARC CPU with 1G of RAM). Although pretty old, the NAS was fitted with a pair of brand-new 2TB WD Reds so data should still be relatively safe. Storage is made available on the LAN through NFS with reasonable transfer speeds.

As for software I prefer dumping full disk images because it makes recovery such a breeze. Of course this approach is feasible only on easily-removable low-capacity media, like MicroSD cards or small SSDs.
With this setup recovers I am able to recover a dead Raspberry Pi with a single shell command and about 15 minutes.
If recovery time was to be absolutely crucial, one could setup something to automatically flash a spare MicroSD every time a new backup is made, reducing recovery to just a MicroSD card swap. The obvious drawback here is the wear out of the spare MicroSD that should then be replaced with a fresh one right after the recovery procedure is done. Still, the small cost of these kind of storage solutions makes this approach viable.

System images are handled by this [bash script](https://github.com/Bonnee/dotfiles/blob/master/.local/bin/backup.sh) I wrote back in [March 2018](https://github.com/Bonnee/dotfiles/commit/cb2b95213ade35f177edd80ec0f5a0a1037d847e). The script has been running weekly on every server/raspi I own (7 at the time of writing this) and it does the following stuff:

1. mount the output storage on a temporary directory
2. backup the local block device to output
3. clean up older backups
4. unmount the output and cleanup the temporary directory

After a few revisions of the script I added compression support via the `-c` option. This helps the system scale to bigger block devices and improves backup speeds on slower networks or on slower machines. With multi-threaded gzip compression enabled I am able to backup my server's 120G SSD in about 2 hours and a half with the NAS being the main bottleneck.

The current script's feature set is:

- Optional compression ([zstd](https://github.com/facebook/zstd), [pigz](https://www.zlib.net/pigz/)/gzip)
- Support for local and remote outputs with automounting
- Auto clean of older backups (it keeps the latest three backups)
- Auto removal of backup image if the backup fails

I'm very satisfied with this whole procedure since it works reliably and runs on every linux box I throw it at.
One future endeavour could be the experimentation of [delta](https://en.wikipedia.org/wiki/Delta_encoding) patches for speeding up the backup process. Basically, instead of taking a full disk image every time, only the bits that changed are taken and are then used to update the previous backup image. I fear this technique will bring excessive complexity though.
