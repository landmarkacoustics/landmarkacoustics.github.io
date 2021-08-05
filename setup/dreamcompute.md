---
title: Set Up a DreamCompute Instance
author: Ben Taft
copyright: 2021 by Landmark Acoustics LLC
---

# Setup for DreamCompute Instances

Log in to [DreamCompute](https://iad2.dreamcompute.com/project/instances/)

## Create a Volume to Store Data

If you create a blank one then you'll have to format it later in the process.

### Launch an Instance

### Chose a Source

I generally prefer the most recent LTS version of Ubuntu.

### Choose a Flavor

I'm still trying to figure out if the 'slowest possible' (my miserly first
instinct) is the righ tway to go here.

### Generate a Private Key

Dreamhost generates the key and lets you download the private key. You must
move it to an appropriate directory and then change the file's permissions.
You must remove the read and write permissions from 'group' and 'others'.

```{console}
mv ~/Downloads KEYFILE /appropriate/directory/KEYFILE
chmod go-rw /path/to/KEYFILE
```

### Start the Instance

Make sure you note the instance's IP address. It will appear in the instances
panel.

## Use the Instance

## Log in:

```{console}
ssh -i path/to/KEYFILE ubuntu@instance_address
```

## Mount a Volume

### Attach the volume to the instance

This is easiest with the web control panel. Once it is attached, you can see
the device path that it is attached at. The path is usually `/dev/vdb`.

### You may need to format the volume

If it's brand new, the volume won't have a file system on it yet. It seems like
'ext4' is the best option for file systems. If that's the case, then you can
format the volume with the `mkfs` command.

```{console}
sudo mkfs.ext4 /dev/vdb
```

### Mount the volume

You need to mount the volume and make the current user (probably 'ubuntu') its
owner. You can also bind the volume to a path in the user's home directory.
That will make it easier to transfer files with `scp`.

```{console}
sudo mkdir -p /mnt/DRIVE_PATH
sudo mount /dev/vdb /mnt/DRIVE_PATH
sudo chown -R $USER:$USER /mnt/DRIVE_PATH
# optional
sudo mount --bind /mnt/DRIVE_PATH ~/PATH/IN/HOME/DIRECTORY
```

### Make it Automatic

The previous section is great the first time that you set up your drive. Once
you know that it works, though, you won't want to have to run all of those
commands manually every time that you start the instance. That's where the
`/etc/fstab` file comes in! Based on the information from [DreamHost][1], you
can add lines to the end of that file that basically emulate the `sudo mount`
commands in the previous section. Note the helpful header that explains what
different fields are.

file system
: the source of the mount

mount point
: the location of the mount

type
: probably 'ext4', but you should know it

options
: probably 'defaults', but can be 'bind'

dump
: just leave this as zero

pass
: 0 for bindings, 1 for the root file system, and 2 otherwise

```
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# Don't alter the first two lines! They are critical system resoruces.
LABEL=cloudimg-rootfs	/	 ext4	defaults	0 0
LABEL=UEFI	/boot/efi	vfat	defaults	0 0
# These two lines are the ones that mimic the mnt commands above.
UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx /mnt/DRIVE_PATH ext4 defaults 0 2
/mnt/DRIVE_PATH /home/ubuntu/PATH/IN/HOME/DIRECTORY none bind 0 0
```

## Set Up a `venv`

### make sure the virtual environment toolchain is installed:

```{console}
sudo apt install --upgrade python3-venv
```

### create the virtual environment

Let's say that you have mounted the storage volume at ~/volume, and you want
the virtual environment directory to be called 'venv'.

```{console}
cd ~/volume
python3 -m venv --clear --copies venv
```

If you have already set up the virtual environment, then you activate it this
way:

```{console}
source ~/volume/venv/bin/activate
```

### clone the relevant parts of the LAC monorepo from GitHub

I first started working on this with lacwebsockets' registry server
application, so that's what I will use as an example.

```{console}
git clone --no-checkout https://github.com/sternarubra/LAC.git
git sparse-checkout set /lacwebsockets /public-key-infrastructure
~/volume/venv/bin/pip3 install --upgrade -e lacwebsockets
```

[1]: https://help.dreamhost.com/hc/en-us/articles/221778268-How-to-create-and-mount-volumes-on-DreamCompute
