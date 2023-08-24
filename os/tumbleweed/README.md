openSUSE Tumbleweed

Config: (Refer https://wiki.archlinux.org/title/Silent_boot)

Silent boot and shutdown:

- Add `quiet` and `vt.global_cursor_default=0` to grub kernel boot params.
  
- Add `setterm -cursor on` to `/etc/profile`.
  
- In
  ```
  /lib/systemd/system/getty@tty1.service.d/hide-prompt.conf
  ```
  add the following:
  
  ```
  [Service]
  ExecStart=
  ExecStart=-/usr/bin/agetty --skip-login --nonewline --noissue --noclear %I $TERM
  ```
  
- In
  ```
  /usr/lib/systemd/system/hide-shutdown.service
  ```
  add the following:
  
  ```
  [Unit]
  Description=chvt1

  [Service]
  ExecStart=chvt 1

  [Install]
  WantedBy=shutdown.target
  ```

- Run `sudo systemctl daemon-reload` and `sudo systemctl enable hide-shutdown.service`

- PROFIT!
  
