# Customized Lockscreen on Linux with `i3lock`

![blur + overlay](http://i.imgur.com/PSOoCTZ.jpg)

Figure 1. Blurred current workspace + overlay

---
### Prerequisites
- `scrot` for taking screenshot
- `imagemagick` for image manipulation
- `i3lock` for the main Lockscreen
- Optional: An image (with alpha channel) to overlay the blurred workspace
- Optional: `xautolock` for timeout lock behavior

### The Process

`scrot` captures a snapshot of the current workspace
```bash
  $ scrot /tmp/current.png
```
`imagemagick` then processes it into the lockscreen image
```bash
  // adding blur
  $ convert -blur 0x4 /tmp/current.png /tmp/blur.png

  // adding overlay
  $ composite -gravity southeast ./overlay.png /tmp/blur.png /tmp/lock.png
```

> **Notes**

> - The syntax for `composite` is [overlay] - [destination] - [final product].

> - `-gravity` takes `center` and compass directions as arguments (e.g. `north`, `southwest`). For more precise positioning, look up the `-geometry` flag for `imagemagick`.

Lastly, `i3lock` applies the lock using the final product (blurred screenshot with overlay)

```bash
  $ i3lock -i /tmp/lock.png
```

### How to use the script
- Modify the script to point to the correct overlay image(s)
- Symlink or copy it to `/bin`
- Give it executable privilege
```bash
  $ sudo chmod +x /bin/v3lock
```
- Optional: Apply a keybind to activate the script, for example in `i3-wm`
```bash
  // ~/.config/i3/config
  bindsym Mod1+Control+l  exec /bin/v3lock
```
- Optional: add `xautolock` to your startup script (rc.lua, .bashrc, .xsessionrc, etc.)
```bash
  // ~/.xinitrc
  xautolock -time 10 -locker v3lock
```
