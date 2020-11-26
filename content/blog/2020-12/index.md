---
title: "Monthly journal, December 2020"
date: 2020-11-23T16:08:51+01:00
draft: true
---

## Timelapse

![Current webcam snapshot](https://www.villaclara.it/images/webcam/webcam.php)
I have a webcam on my roof, pointed at [Lake Garda](https://en.wikipedia.org/wiki/Lake_Garda). I always wanted to have a timelapse of the footage, but didn't have a server at the time to constantly fetch images from the camera. During the years I tried different methods, like using PHP running on a hosted webserver to fetch the images and construct a video. To no one's surprise, that approach didn't work that well.\
I remember also toying with bash scripts on a Raspberry Pi, but I lost interest at some point and never revisited the problem again.

Last week I came up with the idea of using CI/CD, so I created [this](https://github.com/Bonnee/timelapse) repo that, leveraging [GitHub Actions](https://github.com/features/actions), fetches images from a public webcam and composes them into a timelapse of the last 24 hours. Currently, The workflows I created are _Fetch_ and _Timelapse_, which respectively pulls the images using curl every 5 minutes, and creates a timelapse using FFmpeg.\
{{< figure caption="Fetch should run every 5 minutes" src="actions.png" >}}
While testing GH Actions I realized their unreliability (they are free-_ish_ after all): The _Fetch_ workflow sometimes takes over 5 minutes to complete, colliding with the following _Fetch_ call, thus failing to commit because of an out-of-date working branch. Sometimes actions even fail to trigger, leaving gaps between image frames. At first glance the problem doesn't seem to be that bad though.

After a day that a snapshot has been taken, I want to remove it automatically so that the timelapse always shows the last 24 hours. To do this I wrote a simple [shell script](https://github.com/Bonnee/timelapse/blob/master/tools/clean.sh) that, when called inside _Fetch_ deletes old images by checking their file name. Originally I wanted to use git history as a time source but for some reason GH Actions didn't like that.

Overall I'm satisfied with the end result (a bit less with the procedure to make it work though), and unless my inbox gets full of failed action emails I will keep this automation going.

Ah yes, here is the timelapse
{{< video src="https://github.com/Bonnee/timelapse/blob/master/out.mp4?raw=true" >}}
