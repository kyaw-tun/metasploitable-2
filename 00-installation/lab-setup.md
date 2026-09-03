# Installation

This section documents the process of setting up Metasploitable 2 in a QEMU/libvirt environment.

## Downloading Metasploitable 2

Metasploitable 2 was downloaded from SourceForge.

Source: Metasploitable 2

The downloaded archive contained the original Metasploitable 2 virtual machine files.

## Converting the Virtual Machine

The original Metasploitable 2 image was not used directly with my QEMU/libvirt setup, so it had to be converted into a compatible disk image.

Original format: [TBD]

Converted format: [TBD]

Conversion command:

```text
# TODO: reconstruct from terminal history
```

## Importing into QEMU/libvirt

After converting the disk image, the virtual machine was configured and imported into the QEMU/libvirt environment.

Commands/configuration:

```text
# TODO: reconstruct from terminal history
```

## Network Configuration

The Metasploitable 2 VM was connected to the libvirt virtual network.

The VM received the following address during the lab:

```text
192.168.122.68
```

The host-side address of the libvirt network was:

```text
192.168.122.1
```

This allowed the host and Metasploitable 2 VM to communicate over the isolated virtual network.

Troubleshooting

After the initial setup, I encountered additional problems getting the environment working correctly.

## Problem 1 — [TODO]

Symptoms:

TODO

Cause:

TODO

Solution:

TODO

Problem 2 — [TODO]

Symptoms:

TODO

Cause:

TODO

Solution:

TODO

## Verification

Once the setup was working, connectivity to the Metasploitable 2 VM was verified.

```text
# TODO: reconstruct verification command
```

The VM was then ready to be used as the target for the Red Team Essentials coursework.
