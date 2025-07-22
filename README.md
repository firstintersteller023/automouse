### 🖱️ `keep_mouse_active.sh` — Mouse Mover Script (Ubuntu)

#### ✅ Purpose:

Simulates mouse movement using `xdotool` to prevent screen lock/sleep.

```
sudo apt install xdotool
```

---

### 📜 Script: `~/keep_mouse_active.sh`

```bash
#!/bin/bash
while true; do
  xdotool mousemove_relative -- 50 0
  sleep 0.2
  xdotool mousemove_relative -- 0 50
  sleep 0.2
  xdotool mousemove_relative -- -50 0
  sleep 0.2
  xdotool mousemove_relative -- 0 -50
  sleep 0.2
  sleep 60
done
```

---

### ▶️ Start Script in Background

```bash
nohup ~/keep_mouse_active.sh >/dev/null 2>&1 &
```

---

### ⏹️ Stop the Script

```bash
pkill -f keep_mouse_active.sh
```

### Fix Isuues -->
```
---

## ❌ `xdotool` **will not work** on Wayland.

No amount of:

* `nohup`
* `DISPLAY=:0`
* Running in background or as root

will make `xdotool` move the mouse under Wayland. This is by design — Wayland **blocks simulated input** to prevent cheating tools and keyloggers.

---

## ✅ Only Real Fix: **Switch to X11 Session**

To make your current `xdotool` script work (e.g. to fake mouse movement for Hubstaff), you **must**:

### 🔁 One-time permanent fix:

1. Edit GDM config:

```bash
sudo nano /etc/gdm3/custom.conf
```

2. Find this line:

```bash
#WaylandEnable=false
```

3. Uncomment it so it becomes:

```bash
WaylandEnable=false
```

4. Save & Exit (`Ctrl+O`, `Enter`, then `Ctrl+X`)

5. Reboot:

```bash
sudo reboot
```

---

## ✅ After reboot:

Run this to confirm:

```bash
echo $XDG_SESSION_TYPE
```

It should now say:

```bash
x11
```

Now your original script will work:

```bash
nohup ~/keep_mouse_active.sh >/dev/null 2>&1 &
```

---

## ✅ Optional: Autostart on login (if needed)

If you want it to auto-run on login:

1. Create a `.desktop` file:

```bash
nano ~/.config/autostart/mouse-wiggle.desktop
```

2. Paste:

```ini
[Desktop Entry]
Type=Application
Exec=/bin/bash /home/tejas/keep_mouse_active.sh
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Name=Mouse Wiggle
```

3. Save & close

---

Let me know if you want to convert this into a GUI app or daemon.

```
---

