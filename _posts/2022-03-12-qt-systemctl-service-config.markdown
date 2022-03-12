---
layout: post
title:  "Linux Qt软件 systemctl开机自启动 (systemctl service config for Qt app)"
date:   2022-03-12 17:36:00 -0600
categories: raspberry pi, shadowsocks
---

用systemctl 启动安全又方便，但是对于Qt 软件，常用的 .service 配置启动会出错。 

## 用例
- 软件（qbittorrent)

```
[Unit]
Description=qBittorrent Daemon Service
Documentation=man:qbittorrent
Wants=network-online.target
After=network-online.target nss-lookup.target multi-user.target  xdg-desktop-autostart.target

[Service]
Type=simple
Environment = "DISPLAY=:0"
User=pi
ExecStart=/usr/bin/qbittorrent

[Install]
WantedBy=multi-user.target
```

## 解释
关键点：
1. 确保desktop启动： xdg-desktop-autostart.target
2. 配置 DISPLAY 和 Xauthority (指定了user 就不用 再指定 xauthority path)
```
Environment="DISPLAY=:0"
Environment="XAUTHORITY=/home/pi/.Xauthority"
```

## 参考
1. https://www.kryii.com/12.html
2. https://forum.qt.io/topic/133449/starting-qt-app-as-a-systemd-service/16
3. https://systemd.io/DESKTOP_ENVIRONMENTS/