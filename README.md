# cloudkey

**cloudkey** is a replacement for `/usr/bin/ck-ui` on your Ubiquity Cloud Key
Generation 2 device.

![screenshot](https://raw.githubusercontent.com/jnovack/cloudkey/master/doc/screenshot.gif)

*Note: Delay is slowed down to show fading between screens.*

## Installation

### Quick Start

1. `ssh ubnt@UniFi-CloudKeyG2`
2. `mv /usr/bin/ck-ui /usr/bin/ck-ui.original`
3. `curl -Lo /usr/local/ck-ui LINK_FROM_RELEASES_PAGE`

### Compile in Docker

1. `docker run --rm -it golang`
2. `git clone https://github.com/jnovack/cloudkey.git`
3. `go env -w GO111MODULE=auto`
4. `cd cloudkey`
5. `go mod init github.com/jnovack/cloudkey`
6. `go mod tidy`
7. `GOOS=linux GOARCH=arm go build cloudkey.go`

### Developers

1. Have a working Go environment.
2. `GOOS=linux GOARCH=arm go build cloudkey.go`
3. SCP the file over to your Cloud Key.

At this point, you can choose to backup and overwrite the `/usr/bin/ck-ui`
file or create a new systemd service, depending on your linux experience.

#### Using the `systemd` Service

Disable the old service first.

1. `systemctl disable ck-ui`
2. `systemctl stop ck-ui`

Install this one.

1. scp `cloudkey.service` to the `/lib/systemd/system/` directory.
2. `touch /etc/cloudkey.env`
3. `systemctl enable cloudkey`
4. `systemctl start cloudkey`

## Why?

I am an edge case.  I do not use my Cloud Key device for Unifi.  I think it is
a great sexy little hardware device, but to manage a network off of what is
essentially a POE SDCard, you are insane.

Issues with stability are [very well documented](https://help.ubnt.com/hc/en-us/articles/360000128688-UniFi-Troubleshooting-Offline-Cloud-Key-and-Other-Stability-Issues#4).
Using mongodb on an sdcard (limited write cycles) without *automatically*
reparing has lead me to have to recover 4 times in 2 years even with the
secondary USB power from the UPS. That is NOT remotely production stable.
Run Unifi on a server, not a "raspberry pi".

With that said, I am sure you are asking yourself *"Why do you have it all?"*
The Ubiquity Cloud Key Gen2 is a POE, ARMv7, Single-Board-Computer with
on-board battery backup and a 160x60 framebuffer display built-in.  It is
sexy, for under $200. It looks like an iDevice.

Sure, you can buy a $35 Raspberry Pi, add a case, with a touchscreen, with
a power-supply, and blah blah, but I'll pay for quality and craftmanship so
it does not look like another Frankenstein project around my house.

I can ship it to my parents, tell them to plug one cable into the new-fangled
doo-hickey and tell them to call their ISP when it has a sad face on it
(feature not developed yet).
