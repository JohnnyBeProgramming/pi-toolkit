# Rasberry Pi Toolkit

A set of scripts and tooling to remotely setup and control Rasberry Pi's.

## Main features & functionality

The tooling and functionality included:

- ./setup - Scripts used to install various pieces of software
- ./remote - Some helper scripts to remotely control a Pi through ssh
- ./backups - Backups will be stored here by default

## Quickstart

One of the first things to do, is to set up and enable `ssh` on a `Rasberry Pi`.
This enables us to remotely control the device, and perform actions such as
backup and restore.

### Setup SSH on the Rasberry Pi

Run the following command to install ssh, so we can remotely access the device.
Note that this will require an active internet connection.

```
# Exposes an ssh server on the device
bash <(curl -s curl https://raw.githubusercontent.com/JohnnyBeProgramming/pi-toolkit/master/setup/ssh)
```

### Remotely control device from your machine

To do this, we will make use of the SSH connection that we installed and enabled
on the device.

```
# Test the connection from your host to the remote device
# Hint: Use the IP that was resolved in the step above

ssh <user>@<device-ip-addr>
# Example: ssh pi@192.168.1.61 hostname

exit # Type 'exit' to terminate the ssh session...
```

Now that we established a connection, the next step would be to try and remotely 
install the required features, and set up any other funtionality we need.

```
# Checkout this repo on your local machine
git clone https://github.com/JohnnyBeProgramming/pi-toolkit.git
cd pi-toolkit

# Remotely sync this folder to the target device (ignores the `./backup` folder)
./remote/sync-to pi@<your-device-ip> <device-path> <local-path>

# Sync files from the device to a local folder
./remote/sync-from pi@<your-device-ip> <device-path> <local-path>

# Defaults to `/home/pi/` -> `./devices/pi@<your-device-ip>/home/pi/`
./remote/sync-from pi@192.168.1.61 /home/pi/ xxx

# Make a backup of the target device (stored in the backups folder)
./remote/backup

```