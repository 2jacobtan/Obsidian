
[[WSL2 Ubuntu setup]] (personal)

---

https://github.com/mintty/wsltty

---

### GUI support for WSL2

Download and install:
https://sourceforge.net/projects/vcxsrv/ (for GUI support)
- to be superseded by https://github.com/microsoft/wslg (with new Windows version, sometime 2nd quarter 2021, hopefully)


```bash
sudo apt update
sudo apt install ubuntu-desktop
```


Firewall must allow `VcXsrv windows xserver` to communicate on both <u>private</u> and <u>public</u> networks.

add to ~/.zshrc (for Zsh users; ~/.bashrc for Bash users)
```bash
# VcXsrv
export DISPLAY=$(/sbin/ip route | awk '/default/ { print $3 }'):0
export LIBGL_ALWAYS_INDIRECT=0
# export LIBGL_ALWAYS_SOFTWARE=1
```

Cf. https://stackoverflow.com/questions/61110603/how-to-set-up-working-x11-forwarding-on-wsl2

Restart shell by `exec zsh` (for Zsh users; `exec bash` for Bash users). Or just open a new terminal.

On Windows: open `XLaunch` .

On the "Extra settings" page of the wizard, tick "Disable access control".

![](https://i.stack.imgur.com/6C7AT.png)