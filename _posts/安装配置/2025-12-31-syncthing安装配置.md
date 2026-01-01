---
layout: post
title: syncthing安装配置
date: 2025-12-31 01:50 +0800
categories: ['安装配置']
tags: ['syncthing']
---
# 安装
## Ubuntu
先按照[这里](https://apt.syncthing.net/)把syncthing安装上

然后创建并编辑`/etc/systemd/system/syncthing@.service`文件。[官方模板](https://github.com/syncthing/syncthing/blob/main/etc/linux-systemd/system/syncthing%40.service)
```systemd
[Unit]
Description=Syncthing - Open Source Continuous File Synchronization for %I
Documentation=man:syncthing(1)
After=network.target
StartLimitIntervalSec=60
StartLimitBurst=4

[Service]
UMask=0027 # 此行非官方模板，用户将同步过来的文件权限放到用户组里，方便其他用户使用
User=%i
Environment="STLOGFORMATTIMESTAMP="
Environment="STLOGFORMATLEVELSTRING=false"
Environment="STLOGFORMATLEVELSYSLOG=true"
ExecStart=/usr/bin/syncthing serve --no-browser --no-restart
Restart=on-failure
RestartSec=1
SuccessExitStatus=3 4
RestartForceExitStatus=3 4

# Hardening
ProtectSystem=full
PrivateTmp=true
SystemCallArchitectures=native
MemoryDenyWriteExecute=true
NoNewPrivileges=true

# Elevated permissions to sync ownership (disabled by default),
# see https://docs.syncthing.net/advanced/folder-sync-ownership
#AmbientCapabilities=CAP_CHOWN CAP_FOWNER

[Install]
WantedBy=multi-user.target
```
{: file="/etc/systemd/system/syncthing@.service"}

最后启动服务即可:
```bash
systemctl enable syncthing@syncthing.service
systemctl start syncthing@syncthing.service
```

## MacOS
```zsh
brew install syncthing
brew services start syncthing
```
