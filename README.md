# ORDS Systemd Service Setup

This repository provides instructions to configure Oracle REST Data Services (ORDS) as a systemd service on a Linux system. By setting up ORDS as a service, it can automatically start at boot, restart on failure, and be easily managed with systemd.

## Prerequisites

- Oracle REST Data Services (ORDS) installed and configured.
- Java installed on the system.
- Root or sudo access to manage system services.
- ORDS path and configuration details available.

## Repository Structure

```bash
ords-systemd-service/
├── scripts/
│   ├── install.sh          # Script to automate service setup
│   ├── ords.service        # Systemd service file for ORDS
├── README.md               # This documentation
```

## Setup Steps
1. Clone the Repository
First, clone this repository to get the necessary files:
```bash
git clone https://github.com/your-repo/ords-systemd-setup.git
cd ords-systemd-service
```

## Configure the ords.service File
Open the scripts/ords.service file and edit the following settings:

ExecStart: Set this to the full path of your ORDS binary. Example:
```bash
ExecStart=/home/username/ords-latest/bin/ords
```

User: Set the user that will run the ORDS service. Example:
```bash
User=ords_user
```

WorkingDirectory: Set the working directory where ORDS is installed.


##  Copy the Service File to Systemd
Run the following command to copy the ords.service file to the systemd service directory:

```bash
sudo cp scripts/ords.service /etc/systemd/system/
```
##  Commands to Manage the ORDS Service
These commands are necessary to manage the ORDS systemd service.

Edit the ORDS service file:

Use this command to edit the ORDS systemd service file if needed.

```bash
sudo vim /etc/systemd/system/ords.service
```

Reload the systemd daemon:

Run this command to reload the systemd daemon and apply any changes made to the service file:

```bash
sudo systemctl daemon-reload
```
Start the ORDS service:

This command starts the ORDS service:

```bash
sudo systemctl start ords.service
```
Check the status of the ORDS service:


```bash
sudo systemctl status ords.service
```
View recent logs for the ORDS service, this command displays the last 200 log entries for the ORDS service:

```bash
sudo journalctl -u ords.service -n 200
```

##  Testing the ORDS Service
Once the service is running, you can test it using a simple curl command. Replace username and password with your ORDS credentials and 2121_db121p with your pool name

```bash
curl -i --user username:password http://localhost:8087/ords/2121_db121p/hr/_/sql \
--header 'Content-Type: application/sql' \
--data-raw 'SELECT * FROM DUAL;'
This will send a SQL query to the ORDS service running on localhost at port 8087 and execute a basic query (SELECT * FROM DUAL;).
```

##  Additional ORDS Service Management
You can stop or restart the ORDS service using the following commands:

Stop the ORDS service:

```bash
sudo systemctl stop ords.service
```

Restart the ORDS service:

```bash
sudo systemctl restart ords.service
```
Troubleshooting
Check logs with the journalctl command or in /var/log/syslog.
Make sure the ORDS binary path is correct in the service file.
Ensure that ORDS is correctly configured and has the necessary permissions.
Uninstallation
To remove the ORDS service, stop the service and disable it from starting on boot:

```bash
sudo systemctl stop ords
sudo systemctl disable ords
```
Then remove the service file:

```bash
sudo rm /etc/systemd/system/ords.service
```
Reload the systemd daemon to apply the changes:

```bash
sudo systemctl daemon-reload
```




