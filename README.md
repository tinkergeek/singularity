# singularity

This repository contains Singularity build definitions that I've created for various uses.

## Easy path to Singularity build VM

The best scenario is to use Singularity Hub but...

Some users need a helping hand getting to a place where they can build their own Singularity images. This tidbit describes getting users a generic virtual machine where they can be root.

I provide the libguestfs.img container (libguest.def included here):
```
# Build image with a recent libguestfs install
singularity build libguestfs.img libguestfs.def
```

Users are then free to root their favorite distribution's pre-made images:
```
# Download pre-made Cloud Image
wget http://cloud.centos.org/centos/7/images/CentOS-7-x86_64-GenericCloud.qcow2c
# or Fedora..
# wget https://download.fedoraproject.org/pub/fedora/linux/releases/27/CloudImages/x86_64/images/Fedora-Cloud-Base-27-1.6.x86_64.qcow2
# or ubuntu..
# wget https://cloud-images.ubuntu.com/xenial/current/xenial-server-cloudimg-amd64-disk1.img
# Make libguestfs launch a qemu instance and not libvirtd
export LIBGUESTFS_BACKEND=direct
# Reset root password
singularity exec --bind /run libguestfs.img -- virt-customize -a CentOS-7-x86_64-GenericCloud.qcow2c --root-password password:mysecurepassword1
```

Example using qemu without root permissions:
```
# Now run your new VM (chmod 666 /dev/kvm)
qemu-system-x86_64 -cdrom CentOS-7-x86_64-GenericCloud.qcow2 -vnc :0 -enable-kvm -smp 1 -m 2048 -netdev user,id=net1 -device e1000,netdev=net1
# Connect to VM over VNC
vncviewer :0
```

