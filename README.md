# proxmox-plex

run the following

``` shell
bash <(curl -s https://raw.githubusercontent.com/shepner/proxmox-blueiris/master/proxmox/create_vm.sh)
```

Follow the installer steps until you reach the installation type selection where you need to select "Custom (advanced)"

Now click "Load driver" to install the VirtIO drivers for hard disk and the network:

* Hard disk: Browse to folder `vioscsi\w10\amd64` and install the driver
* Network: Browse to folder `NetKVM\w10\amd64` and install the driver
* Memory Ballooning: Browse to folder `Balloon\w10\amd64` and install the driver 

Choose the drive and continue the Windows installer steps.

After Windows starts, install the "Qemu Guest Agent". The installer is located on the driver CD under `guest-agent\qemu-ga-x86_64.msi`








provide a static IP address

DNS: 10.0.0.5,208.67.222.222,208.67.220.220

Do NOT setup disk as LVM group

install OpenSSH

## Fix the UID

First set the permissions of the home dir:

``` shell
sudo chown -R 1001 /home/`id -un`
```

Then change the UID accodingly in the passwd files:

``` shell
sudo vipw
```

Finally, logout and back in again

## Setup ssh keys

Do this from the local workstation:

``` shell
DHOST=plex
ssh-copy-id -i ~/.ssh/shepner_rsa.pub $DHOST

scp ~/.ssh/shepner_rsa $DHOST:.ssh/shepner_rsa
scp ~/.ssh/shepner_rsa.pub $DHOST:.ssh/shepner_rsa.pub
scp ~/.ssh/config $DHOST:.ssh/config
ssh $DHOST "chmod -R 700 ~/.ssh"
```

## Configure the system

``` shell
bash <(curl -s https://raw.githubusercontent.com/shepner/proxmox-plex/master/update-scripts.sh)

~/update.sh

~/scripts/setup/userConfig.sh
~/scripts/setup/systemConfig.sh
~/scripts/setup/nfs.sh
~/scripts/setup/smb.sh
~/scripts/setup/plex.sh
```
