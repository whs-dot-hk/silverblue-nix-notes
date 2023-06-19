```sh
sudo mkdir -p /var/nix
```
```txt
# /etc/systemd/system/nix.service
[Unit]
Description=Create nix directory
Before=local-fs-pre.target
Wants=local-fs-pre.target

[Service]
Type=oneshot
ExecStartPre=chattr -i /
ExecStart=/bin/sh -c "mkdir -p /nix && chown -R whs /nix"
ExecStopPost=chattr +i /

[Install]
WantedBy=local-fs.target
```
```txt
# /etc/systemd/system/nix.mount
[Unit]
Description=Mount nix
After=var.mount
Wants=var.mount

[Mount]
What=/var/nix
Where=/nix
Options=bind

[Install]
WantedBy=local-fs.target
```
