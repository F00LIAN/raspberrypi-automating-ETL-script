<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/d/d1/Raspberry_Pi_OS_Logo.png" alt="raspberrypi-logo" width="500"/>
</p>

# Fully Automate ETL Script on Raspberry Pi OS

Automate your ETL (Extract, Transform, Load) processes seamlessly on Raspberry Pi OS using systemd services and timers. This project provides a cost-effective and efficient solution for continuous data automation tasks without relying on virtual machines or cloud resources.

## 📹 Video Demonstration

- ### [YouTube: Automate Python ETL Script on Raspberry Pi](https://www.youtube.com/watch?v=K026myVD4og)

## 🛠️ Environments and Technologies Used

- **Hardware:** Raspberry Pi Device
- **Software:** 
  - Raspberry Pi OS
  - Linux Terminal
  - SSH
  - VIM
  - VS Code
  - Bash Scripting
  - Python 3

## 💻 Operating Systems Used

- Raspberry Pi OS
- Windows 11

## 🚀 High-Level Deployment and Configuration Steps

1. **Set Up Raspberry Pi & Install Operating System**
2. **Develop the ETL Automation Script**
3. **Configure SystemD Service and Timer**
4. **Deploy and Monitor the Automated ETL Service**

---

# Deployment and Configuration Steps

## 🔐 SSH into Raspberry Pi

<p align="center">
  <img src="(https://github.com/user-attachments/assets/8c3548b9-179d-482a-b72b-3ab2a6cc2c25)
" height="80%" width="80%" alt="SSH into device over LAN"/>
</p>

Begin by connecting to your Raspberry Pi via SSH over your local network. Ensure that SSH is enabled and that you can successfully log in to your device. This remote access is crucial for managing and deploying your ETL scripts without needing a direct monitor or keyboard connection.

## 📄 Create ETL Automation Script and Shell Command. Copy and Paste into RaspberryPi

<p align="center">
  <img src="(https://github.com/user-attachments/assets/f3499e92-a721-4005-9657-5d50e4e5541c)
" height="80%" width="80%" alt="Script Development in VSCode"/>
</p>

Develop your ETL automation script using VIM or your preferred text editor. This script will handle data extraction, transformation, and loading processes. Copy and paste the entire project folder into the RaspberryPi. Ensure the script has executable permissions and is tested manually before integrating it with the RaspberryPi.

<p align="center">
  <img src="(https://github.com/user-attachments/assets/80c57cd6-5ba1-44c4-95e9-4d60f043e1a8)
" height="80%" width="80%" alt="Script Development in VIM"/>
</p>

Add a shell command to initiate on the raspberrypi for when we setup the automation service. 

## 📄 Make Script Executable 
```bash
chmod +x ~/scripts/auto_etl.sh
```

## 🛠️ Configure SystemD Service and Timer

### 📂 Create the Service File

![image](https://github.com/user-attachments/assets/3f350eb6-4f0e-490f-a19c-614257015847)

Open the service file for editing:

```bash
sudo nano /etc/systemd/system/auto-etl.service
```

Add the following content:

```ini
[Unit]
Description=Automated ETL Service
After=network.target

[Service]
Type=oneshot
ExecStart=/home/pi/scripts/auto_etl.sh
User=pi
Group=pi
Restart=on-failure
Environment=PATH=/usr/bin:/bin

[Install]
WantedBy=multi-user.target
```

### ⏲️ Create the Timer File

![SystemD Timer Configuration](https://github.com/user-attachments/assets/e901296e-9cc2-43de-9df4-12f3f07a3a8a)

Open the timer file for editing:

```bash
sudo nano /etc/systemd/system/auto-etl.timer
```

Add the following content:

```ini
[Unit]
Description=Run ETL Service Every 3 Days

[Timer]
OnBootSec=5min
OnUnitActiveSec=3d
Unit=auto-etl.service
Persistent=true

[Install]
WantedBy=timers.target
```

### 🔄 Reload SystemD and Enable Timer

Reload systemd to recognize the new unit files:

```bash
sudo systemctl daemon-reload
```

Enable the timer to start on boot:

```bash
sudo systemctl enable auto-etl.timer
```

Start the timer immediately:

```bash
sudo systemctl start auto-etl.timer
```

## 📈 Deploy and Monitor the Automated ETL Service

![Monitoring Service Logs](https://github.com/user-attachments/assets/515aee56-3538-474c-9b11-7d33fd5c56d1)

### Check Timer Status

```bash
systemctl status auto-etl.timer
```

### Check Service Logs

```bash
journalctl -u auto-etl.service -f
```

### Review Script Logs

```bash
cat /home/pi/logs/auto_etl.log
```

### List All Timers

```bash
sudo systemctl list-timers --all
```

## 📜 Detailed Steps

### Step 1: Set Up Raspberry Pi & Install Operating System

1. Download the latest Raspberry Pi OS image
2. Use tools like Balena Etcher to flash the OS onto your SD card
3. Insert the SD card into your Raspberry Pi and boot up
4. Connect to your network via Ethernet or Wi-Fi
5. Enable SSH by creating an empty ssh file in the boot partition

### Step 2: Develop the ETL Automation Script

Create a directory for your scripts:

```bash
mkdir -p ~/scripts
```

Create and edit your ETL automation script:

```bash
nano ~/scripts/auto_etl.sh
```

Make the script executable:

```bash
chmod +x ~/scripts/auto_etl.sh
```

### Step 3: Configure SystemD Service and Timer

Follow the configuration steps outlined above for creating and enabling the service and timer files.

### Step 4: Deploy and Monitor the Automated ETL Service

Follow the monitoring steps outlined above to ensure proper operation of your automated ETL service.

## 🏁 Conclusion

By following this tutorial, you have successfully automated your ETL processes on a Raspberry Pi using systemd services and timers. This setup ensures that your data extraction, transformation, and loading tasks run reliably at scheduled intervals without manual intervention. Leveraging the power of Raspberry Pi OS and systemd provides a robust and cost-effective automation solution for your data workflows.






