# Rasberry Pi Toolkit

A set of scripts and tooling to remotely setup and control Rasberry Pi's.

## Main features & functionality

The tooling and functionality included in this repository:

- ./setup - Scripts used to install various pieces of software
- ./remote - Some utility scripts to remotely control a device through ssh
- ./backups - Backups will be stored here by default

## Quickstart

The following quickstart guides will help you to get you going.

### Run remote scripts from a github repo (on your machine or Rasberry Pi )

To save time and effort by not having to type endless commands into a terminal,
we can make use of statically hosted scripts that are included in this repo.

```
export GITHUB_URL=https://raw.githubusercontent.com/JohnnyBeProgramming/pi-toolkit/master
export TARGET_HOST=pi@192.168.1.1
export DEVICE_PATH=/home/pi
export LOCAL_PATH=./exported

# Install applications & tools from remote scripts...
curl ${GITHUB_URL}/setup/ssh | bash
curl ${GITHUB_URL}/setup/transmission | bash

# Make a backup of a remote device
curl ${GITHUB_URL}/remote/backup | bash

# Sync file to and from a device
curl ${GITHUB_URL}/remote/sync-from | bash -s ${TARGET_HOST} ${DEVICE_PATH} ${LOCAL_PATH}
curl ${GITHUB_URL}/remote/sync-to   | bash -s ${TARGET_HOST} ${DEVICE_PATH} ${LOCAL_PATH}

```

### Setup an SSH agent on the Rasberry Pi

This enables us to remotely control the device, and perform actions such as
backup and restore. Note that this will require an active internet connection.

Run the following command to install ssh:
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
# Example: ssh pi@192.168.1.1 hostname

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
./remote/sync-from pi@192.168.1.1 /home/pi/ xxx

# Make a backup of the target device (stored in the backups folder)
./remote/backup

```