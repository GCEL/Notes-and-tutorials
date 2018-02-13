# Sharing data between the Virtual Machine and the Host Machine

These are some notes on how to transfer data between a virtual machine running on your computer within VirtualBox. 

Lets say you have a virtual machine with a Linux (Ubuntu) operating system. This is the **guest** operating system, or the _guest machine_. The guest machine runs in VirtualBox which is installed on the **host** machine, or host operating system. The host is your physical, real computer or laptop. #

## Install "guest additions" add-on for Ubuntu

Firstly, make sure your guest (Ubuntu) virtual machine is up and running. You need to first install the following packages:

```
sudo apt-get install dkms build-essential linux-headers-generic zserver-org xserver-org-core
```

The latest versions of VirtualBox come with an add-on called _Guest Additions_. This is a CD image that is bundled with VirtualBox. You need to install the Guest Additions on your ***guest*** operating system. You can do this by going starting up the virtual machine, and waiting for it to boot up. In the Guest OS window, go to the `Devices > Install Guest Additions CD image...` menu item. It will mount the CD image on the Ubuntu VM. It may ask to start the softweare automatically, I prefer to start it manually by navigating in the terminal to the CD image location and running the install script as follows:

```
cd /media/USER/cd-mount-point
sudo ./VBoxLinuxAdditions.run
```

After installation has completed, reboot the Ubuntu Virtual Machine. 

## Sharing a folder between host and guest OS

First, you need to make a folder on the host machine (e.g. Windows/MacOS/ or another linux OS). We can call it `virtual`. In linux, do:

```
mkdir ~/virtual
```

(Or wherever you want the shared folder to be on the Host.)

Now, back on the **guest** virtual machine window, click on `Devices > Shared Folders > Shared Folders Settings`

1. Click the little `add` icon under Shared Folders, and navigate to where you created the `virtual` folder on the Host OS file system. Give the folder the same name. You may wish to check the `Auto-mount` and `Make permanent` options here to auto mount the folder (this does not always work, but you can try it.)

2. Create a folder with the same name on the guest os:

```
mkdir ~/virtual
```
(creates a folder called `virtual` in the guest OS home directory. (You can put it somewhere else if you like).

### Extra steps if folder does not automatically appear in the Ubuntu guest OS

3. If the folder is not automatically mounted in Ubuntu, we need to mount the folder on the guest OS (Ubuntu). (You may need to reboot the guest OS here if the shared drive does not automatically appear).

```
sudo mount -t vboxsf virtual ~/virtual
```
In this example, we are mounting the virtual folder on the host to `~/virtual`, the folder we just created a second ago.

If you want to mount the folder without root ownership (so you don't have to type `sudo` every time to modify it, you can mount it with:

```
sudo mount -t vboxsf -D uid=1000, gid=1000, virtual ~/virtual
```

If you mounted the folder as root initially, and wish to change this to your username, you can run:

```
sudo chown USER:GROUP ~/virtual
sudo chmod +rw ~/virtual
```

_(If you are not sure of your user and group name, run `users` at the terminal.)_

To mount the folder every time at start up, we need to add to the `/etc/fstab` table in the Ubuntu guest OS.

First we need the UUID of the virtual folder. Run `blkid`:

```
sudo blkid
```

_(You may need to install the package `libblkid1`, e.g. `sudo apt-get install libblkid1`.)_

Look for the long alpha-numeric string (UUID) next to the `virtual` drive. Copy this to the clipboard.

Now open the `fstab` file:

```
sudo gedit /etc/fstab
```

Add the following line to the `fstab` table:

```
UUID=[PASTE THE UUID HERE]    ~/virtual   auto   rw,usr,auto   0   0
```

It should now automatically mount after a reboot of the Ubuntu guest OS.




