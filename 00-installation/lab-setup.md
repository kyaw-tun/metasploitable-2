# Installation

The course sets up the Red Team Essentials lab using two virtual machines: Kali Linux and Metasploitable 2. The host machine used in the course is Windows, and VMware is used as the virtualization platform.

The installation process in the course can be summarized as:

1. Install Kali Linux as a virtual machine.
2. Download and install Metasploitable 2 as a second virtual machine.
3. Configure the virtual network so that Kali and Metasploitable 2 can communicate with each other.
4. Use Kali as the attacker machine and Metasploitable 2 as the intentionally vulnerable target.

## My Setup

My environment was different from the one used in the course. I already had Kali Linux installed directly as my host operating system, so I did not need to create a Kali virtual machine.

Instead, I only needed to install Metasploitable 2 as a virtual machine. Rather than using VMware as in the course, I used QEMU/KVM with libvirt.

Therefore, my lab setup was:

```text
Course:

Windows host
    │
    └── VMware
         ├── Kali Linux
         └── Metasploitable 2


My setup:

Kali Linux host
    │
    └── QEMU/KVM + libvirt
         └── Metasploitable 2
```

Because the Metasploitable 2 download provides a pre-built VMware-compatible VMDK disk image, an additional conversion step was needed before importing it into my QEMU/libvirt environment.

## Network Isolation

Because Metasploitable 2 is intentionally vulnerable, the course recommends keeping it on an isolated network.

In the VMware setup used in the course, the network configuration is:

- Adapter 1: Host-only Adapter
- Network: Host-only Adapter

This allows the Kali and Metasploitable 2 virtual machines to communicate with each other without placing the vulnerable target directly on the external network.

My setup uses libvirt's `default` virtual network instead. The `default` network is backed by the `virbr0` virtual bridge.

The important principle is the same: Metasploitable 2 should be kept on a controlled lab network and should not be exposed directly to an untrusted or external network.

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

For my QEMU/libvirt setup, I converted the VMDK to QCOW2, a disk-image format commonly used with QEMU/libvirt:

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

Create the VM using `virt-install`:

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

In this setup, the host-side virbr0 address is `192.168.122.1`, while Metasploitable 2 received `192.168.122.68`.

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

## Notes

Metasploitable 2 also does not always respond to a normal ACPI shutdown request. If a graceful shutdown with:

```bash
sudo virsh shutdown metasploitable2
```

does not work, the VM can be stopped with:

```bash
sudo virsh destroy metasploitable2
```

`virsh destroy` immediately stops the virtual machine rather than performing a graceful guest shutdown, so it should be treated as a last resort.
