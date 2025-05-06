# step-to-auto-start-executable-file-linux-Ubuntu-18.04---aarch64-ARM-
This guide explains how to configure the executable file to automatically start on boot or after a power failure on a Linux 4.9.253-tegra system using `systemd`.
Here’s a clean, well-structured `README.md` file for setting up the **TrafikSolVASD** application as a systemd service on a **Linux 4.9.253-tegra (Ubuntu 18.04, aarch64)** system:

---

````markdown
# 🚦 TrafikSolVASD Auto-Start Service Setup (Ubuntu 18.04 - aarch64)

This guide explains how to configure the TrafikSolVASD application to automatically start on boot or after a power failure on a Linux 4.9.253-tegra system using `systemd`.

---

## ✅ Prerequisites

- use below command & Confirm full path to your application:
 
  ls -ld ~/VSDS1/TrafikSolVASD1/app/dist

-output will be actual path use this in below(WorkingDirectory) accordingly
  /home/aaeon/VSDS1/TrafikSolVASD1/app/dist

````

---

## 🛠 Step-by-Step Setup

### 1. Create a systemd service file

```bash
sudo vi /etc/systemd/system/trafiksolvasd.service
```

Press `i` to enter **insert mode**, then paste the following (adjust paths if necessary):

```ini
[Unit]
Description=TrafikSolVASD Application
After=network.target

[Service]
Type=simple
User=aaeon
ExecStart=/home/aaeon/VSDS1/TrafikSolVASD1/app/dist/TrafikSolVASD
WorkingDirectory=/home/aaeon/VSDS1/TrafikSolVASD1/app/dist
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

To save and exit, press `Esc`, then type:

```
:wq
```

---

### 2. Enable the service

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable trafiksolvasd.service
sudo systemctl start trafiksolvasd.service
```

---

### 3. Verify the service

```bash
sudo systemctl status trafiksolvasd.service
```

If everything is correct, you should see:

```
● trafiksolvasd.service - TrafikSolVASD Application
   Loaded: loaded (/etc/systemd/system/trafiksolvasd.service; enabled)
   Active: active (running)
   ...
```

---

### 📌To **stop** your running service `trafiksolvasd.service`, just run:

```bash
sudo systemctl stop trafiksolvasd.service
```

---

### 📌 If you also want to **disable it from auto-starting on boot**:

```bash
sudo systemctl disable trafiksolvasd.service
```

---

### ✅ To confirm it's stopped:

```bash
systemctl status trafiksolvasd.service
```

You should see:

```
Active: inactive (dead)
```

Let me know if you want to completely remove the service as well.




## 📈 View Application Logs

### Real-time logs

```bash
sudo journalctl -u trafiksolvasd.service -f
```

### Complete log history

```bash
sudo journalctl -u trafiksolvasd.service
```

---

## 💾 Optional: Save Logs to File

Update your service file:

```ini
[Service]
ExecStart=/home/aaeon/VSDS1/TrafikSolVASD1/app/dist/TrafikSolVASD
WorkingDirectory=/home/aaeon/VSDS1/TrafikSolVASD1/app/dist
StandardOutput=append:/home/aaeon/VSDS1/logs/output.log
StandardError=append:/home/aaeon/VSDS1/logs/error.log
Restart=always
RestartSec=5
```

Create the log folder:

```bash
sudo mkdir -p /home/aaeon/VSDS1/logs
sudo systemctl daemon-reload
sudo systemctl restart trafiksolvasd.service
```


## 📂 Why `/etc/systemd/system/`?

This directory is where **custom services** live on `systemd`-based systems. It allows services to:

* Autostart at boot
* Be managed via `systemctl` (start, stop, restart, status)

---

## ✅ Final Result

If you see the service is `active (running)` after reboot:

```bash
sudo systemctl status trafiksolvasd.service
```

You're done! 🎉

---

