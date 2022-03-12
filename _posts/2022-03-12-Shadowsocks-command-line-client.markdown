---
layout: post
title:  "How to use Shadowsocks command line client"
date:   2022-03-12 17:26:00 -0600
categories: raspberry pi, shadowsocks
---

Shadowsocks is a proxy tool. This artical will show some simple config about it.

## Install
Follow offical website:
`sudo apt-get update`
`sudo apt-get install shadowsocks-libev`

## Useage
`ss-local` *[ref: 2]*

## Config file
- default config file
`/etc/shadowsocks-libev/config.json`
- make a copy to edit config file (if you only know url check Shadowsocks URL Scheme for decode)
```
sudo cp /etc/shadowsocks-libev/config.json /etc/shadowsocks-libev/local.json
sudo vim /etc/shadowsocks-libev/local.json
```

## Startup with system by Systemd
- default systemd service
`/lib/systemd/system/shadowsocks-libev-local@.service`
- make a copy
`/lib/systemd/system/shadowsocks-libev-local@local.service`

Check inside the default .service file
```
[Unit]
Description=Shadowsocks-Libev Custom Client Service for %I
Documentation=man:ss-local(1)
After=network-online.target

[Service]
Type=simple
CapabilityBoundingSet=CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_BIND_SERVICE
DynamicUser=true
ExecStart=/usr/bin/ss-local -c /etc/shadowsocks-libev/%i.json

[Install]
WantedBy=multi-user.target
```
It uses %i specifier *[ref: 3, 4]*, so we can just add config file name after @
## Check and enable systemctl service
```
# try start
sudo systemctl start shadowsocks-libev-local@local.service
# check status
sudo systemctl status shadowsocks-libev-local@local.service
# enable
sudo systemctl enable shadowsocks-libev-local@local.service
```



## References
[1. http://claude-ray.com/2018/12/01/ss-local/](http://claude-ray.com/2018/12/01/ss-local/)
[2. https://github.com/shadowsocks/shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev)
[3. https://unix.stackexchange.com/questions/396978/specifier-resolution-i-and-i-difference](https://unix.stackexchange.com/questions/396978/specifier-resolution-i-and-i-difference)
[4. https://www.freedesktop.org/software/systemd/man/systemd.unit.html](https://www.freedesktop.org/software/systemd/man/systemd.unit.html)