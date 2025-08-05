## 🖱️ `keep_mouse_active.sh` — Prevent Screen Lock by Simulating Mouse Movement (Ubuntu 24.04)

### ✅ Purpose:

This script uses `xdotool` to move the mouse in a square pattern every 60 seconds to simulate activity and prevent screen locking or system sleep.

---

```
nohup ./mouse-jiggle.sh >/dev/null 2>&1 &
```

```
pkill -f mouse-jiggle.sh
```

## 📦 1. Install Prerequisite

```bash
sudo apt update
sudo apt install xdotool
```

> `xdotool` only works with **X11**, not Wayland.

---

## 🧠 2. Wayland vs. X11 Support

### ❌ `xdotool` does **NOT** work under Wayland

Wayland **blocks simulated input** (mouse/keyboard) for security reasons.

#### ✅ Solution: Permanently Switch to X11

1. Edit GDM config:

   ```bash
   sudo nano /etc/gdm3/custom.conf
   ```

2. Uncomment the line:

   ```ini
   WaylandEnable=false
   ```

3. Save and reboot:

   ```bash
   sudo reboot
   ```

4. After reboot, confirm you're using X11:

   ```bash
   echo $XDG_SESSION_TYPE
   ```

   ✅ Output should be:

   ```
   x11
   ```

---

## 📜 3. Create Script: `~/keep_mouse_active.sh`

```bash
#!/bin/bash

# Ensure DISPLAY is set for xdotool
export DISPLAY=${DISPLAY:-:0}

while true; do
  # Simulate movement in a square pattern
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

### Make the script executable:

```bash
chmod +x ~/keep_mouse_active.sh
```

---

## ▶️ 4. Run Script in Background

```bash
nohup ~/keep_mouse_active.sh >/dev/null 2>&1 &
```

This will keep it running even after you close the terminal.

---

## ⏹️ 5. Stop the Script

```bash
pkill -f keep_mouse_active.sh
```

---

## 🔁 6. Auto-Start Script on Login (Optional)

1. Create autostart directory if it doesn't exist:

   ```bash
   mkdir -p ~/.config/autostart
   ```

2. Create the `.desktop` file:

   ```bash
   nano ~/.config/autostart/mouse-wiggle.desktop
   ```

3. Paste the following:

   ```ini
   [Desktop Entry]
   Type=Application
   Exec=/bin/bash /home/tejas/keep_mouse_active.sh
   Hidden=false
   NoDisplay=false
   X-GNOME-Autostart-enabled=true
   Name=Mouse Wiggle
   Comment=Prevent screen from locking by moving the mouse
   ```

4. Save and close (`Ctrl + O`, `Enter`, then `Ctrl + X`)

> Make sure the path to your script is correct and matches your username.

---

## 🧪 7. Troubleshooting

* Run manually in a terminal first to confirm:

  ```bash
  ~/keep_mouse_active.sh
  ```

* Check if mouse moves every 60 seconds in a square.

* Ensure you're **logged into a graphical X11 session** — not SSH or Wayland.

---

Let me know if you want:

* A version using `ydotool` (for Wayland + root)
* A GUI toggle applet
* A systemd service instead of `.desktop` file

---
