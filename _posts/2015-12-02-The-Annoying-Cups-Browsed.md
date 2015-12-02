---
layout: post
category: "System"
title: "The Annoying Cups Browsed"
tags: [Debian] [Cups] [Linux]
---
Recently every time I reboot the debian, the screen pauses at a line: `a stop job is running for make remote cups printers available locally` and I must wait 1min30s!!. And I directly goolged this sentence only to find that there are not many people who dicuss it, and this sentence has also grammar error.

<!--more-->

## Point Here

Actually, it is the cups-browsed daemon service that causes problems, because its service desription is exactly `make remote cups printers available locally`, so the message above is "we are stopping cups-browsed service, 1min30s", how can it be!!! 1min30s I already reboot 10 times on a SSD!

For me, cups-browsed is useless, I should disable it from boot to shutdown, so
- `sudo systemctl stop cups-browsed.service` to stop it
- `sudo systemctl disable cups-browsed.service` to disable it
- `sudo systemctl status cups-browsed` or `sudo systemctl is-enabled cups-browsed`to verify
