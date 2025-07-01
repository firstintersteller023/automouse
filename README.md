### 🖱️ `keep_mouse_active.sh` — Mouse Mover Script (Ubuntu)

#### ✅ Purpose:

Simulates mouse movement using `xdotool` to prevent screen lock/sleep.

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

---

