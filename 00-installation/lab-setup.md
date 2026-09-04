# Lab Setup

The course demonstrates the lab setup using Windows + VMware, with Kali Linux and Metasploitable 2 running as virtual machines.

My setup is different:

Host OS: Kali Linux
Virtualization: QEMU/KVM + libvirt
Target VM: Metasploitable 2
Virtual network: libvirt `default` network

Since Kali is already the host OS, I only need to install Metasploitable 2 as a virtual machine.

## 1. Install QEMU/KVM and libvirt

Install the required virtualization packages:

```bash
sudo apt install qemu-system-x86 libvirt-daemon-system virt-manager
```

Add the current user to the `libvirt` and `kvm` groups:

```bash
sudo usermod -aG libvirt,kvm "$USER"
```

After installing and configuring libvirt, verify that the `default` network is available:

```bash
sudo virsh net-list --all
```

Expected output:

```bash
 Name      State    Autostart   Persistent
--------------------------------------------
 default   active   yes         yes
```

The `default` network uses the `virbr0` virtual bridge. Its address can be checked with:

```bash
ip addr show virbr0
```

Example:

```bash
3: virbr0: <BROADCAST,MULTICAST,UP,LOWER_UP> ...
    link/ether 52:54:00:b5:c9:af brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.1/24 brd 192.168.122.255 scope global virbr0
```

## 2. Download and extract Metasploitable 2

Metasploitable 2 is distributed as a pre-built virtual machine image.

After downloading the ZIP archive, create a directory for the VM:

```bash
mkdir -p ~/VMs/metasploitable2
```

Extract the archive:

```bash
unzip ~/Downloads/metasploitable-linux-2.0.0.zip \
  -d ~/VMs/metasploitable2
```

The extracted directory contains the Metasploitable virtual disk:

```bash
~/VMs/metasploitable2/Metasploitable2-Linux/Metasploitable.vmdk
```

Check the disk image:

```bash
qemu-img info ~/VMs/metasploitable2/Metasploitable2-Linux/Metasploitable.vmdk
```

The image is a VMDK with a virtual size of 8 GiB. The actual file is approximately 1.79 GiB because it is a sparse disk image.

## 3. Inspect and Convert the VMDK to QCOW2

The course uses VMware, so the original VMDK can be used directly in that environment.

For my QEMU/libvirt setup, I converted the VMDK to QCOW2, QEMU's native disk-image format:

```bash
sudo qemu-img convert -p \
  -f vmdk \
  -O qcow2 \
  ~/VMs/metasploitable2/Metasploitable2-Linux/Metasploitable.vmdk \
  /var/lib/libvirt/images/metasploitable2.qcow2
```

Verify the converted image:

```bash
sudo qemu-img info /var/lib/libvirt/images/metasploitable2.qcow2
```

Make the disk accessible to the libvirt/QEMU process:

```bash
sudo chown libvirt-qemu:kvm \
  /var/lib/libvirt/images/metasploitable2.qcow2
```
## 4. Create the Metasploitable 2 VM

Create the VM using virt-install:

```bash
sudo virt-install \
  --name metasploitable2 \
  --memory 512 \
  --vcpus 1 \
  --disk path=/var/lib/libvirt/images/metasploitable2.qcow2,format=qcow2 \
  --network network=default,model=e1000 \
  --os-variant generic \
  --import \
  --graphics spice \
  --noautoconsole
```

Important options:

- `--name metasploitable2` — names the virtual machine.
- `--memory 512` — allocates 512 MiB of RAM.
- `--vcpus 1` — allocates one virtual CPU.
- `--disk ...` — attaches the converted QCOW2 disk.
- `--network network=default` — connects the VM to libvirt's `default` network.
- `--import` — uses the existing disk image rather than installing an operating system from installation media.
- `--graphics spice` — provides graphical console access through SPICE.
- `--noautoconsole` — creates/starts the VM without automatically opening a console.

The VM's network interface uses the `e1000` model:

```bash
Interface   Type      Source    Model
----------------------------------------
vnet0       network   default   e1000
```

## 5. Verify the network connection

The libvirt `default` network provides the virtual bridge `virbr0`:

```text
Kali host
192.168.122.1
     │
   virbr0
     │
     │ libvirt default network
     │
   vnet0
     │
Metasploitable 2
192.168.122.68
```

Once the VM is running, its assigned IP can be obtained with:

```bash
sudo virsh domifaddr metasploitable2
```

Example:

```bash
 Name    MAC address         Protocol   Address
-----------------------------------------------------------
 vnet0   52:54:00:ee:ea:1f   ipv4       192.168.122.68/24
```

Verify connectivity from the Kali host:

```bash
ping -c 3 192.168.122.68
```

The target can then be scanned from Kali:

```bash
nmap -Pn 192.168.122.68
```

If both connectivity and scanning work, the Metasploitable 2 lab is ready.

## 6. Default credentials

The default credentials for Metasploitable 2 are:

```text
Username: msfadmin
Password: msfadmin
```

These credentials are intentionally weak because Metasploitable 2 is designed as a vulnerable target for security training.

## Notes

This setup differs from the course's VMware-based setup mainly because the host environment is different. The original Metasploitable 2 disk is provided as a VMDK, so it was converted to QCOW2 before being imported into QEMU/libvirt.

Metasploitable 2 also does not always respond to a normal ACPI shutdown request. If a graceful shutdown with:

```bash
sudo virsh shutdown metasploitable2
```

does not work, the VM can be stopped with:

```bash
sudo virsh destroy metasploitable2
```

`virsh destroy` immediately stops the virtual machine rather than performing a graceful guest shutdown, so it should be treated as a last resort.
