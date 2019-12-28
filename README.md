# Rasberry Pi Toolkit

A set of scripts and tooling to remotely setup and control Rasberry Pi's.

## Main features & functionality

The tooling and functionality included in this repository:

- [./setup](./setup) - Scripts used to install various pieces of software
- [./remote](./remote) - Some utility scripts to remotely control a device through ssh
- [./backups](./backups) - Backups will be stored here by default

## Quickstart

The following quickstart guides will help you to get going.

### Run remote scripts from a github repo (on your machine or Rasberry Pi )

To save time and effort by not having to type endless commands into a terminal,
we can make use of statically hosted scripts that are included in this repo.

```
export REPO_URL=https://raw.githubusercontent.com/JohnnyBeProgramming/pi-toolkit/master

# Install applications & tools from remote scripts...
curl ${REPO_URL}/setup/ssh | bash
curl ${REPO_URL}/setup/transmission | bash

# Make a backup of a remote device
curl ${REPO_URL}/remote/backup | bash

export TARGET_HOST=pi@192.168.1.1
export DEVICE_PATH=/home/pi
export LOCAL_PATH=./exported

# Sync files to and from a device
curl ${REPO_URL}/remote/sync-from | bash -s ${TARGET_HOST} ${DEVICE_PATH} ${LOCAL_PATH}
curl ${REPO_URL}/remote/sync-to   | bash -s ${TARGET_HOST} ${DEVICE_PATH} ${LOCAL_PATH}

```

### Setup an SSH agent on the Rasberry Pi

This enables us to remotely control the device, and perform actions such as
backup and restore, or even remote development (using `vscode`).

Run the following command to install ssh (requires an active internet connection):

```
# Exposes an ssh server on the device
curl -s curl https://raw.githubusercontent.com/JohnnyBeProgramming/pi-toolkit/master/setup/ssh | bash
```

Now you can connect to the remote device using your device username and the `ssh` command:

```
# Test the connection from your host to the remote device
# Hint: Use the IP that was resolved in the step above

ssh <user>@<device-ip-addr>
# Example: ssh pi@192.168.1.1 hostname

exit # Type 'exit' to terminate the ssh session...
```

Now that we established a connection, the next step would be to try and remotely
install the required features, and set up any other funtionality we need.

### Remotely control a device from your local machine

To do this, we will make use of the SSH connection that we installed and enabled
on the device.

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
