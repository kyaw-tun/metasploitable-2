# Nmap

Nmap (Network Mapper) is a network discovery and security auditing tool used to identify hosts, discover open ports, detect services and versions, and perform additional enumeration through NSE scripts.

During the Red Team Essentials course, I used Nmap against the Metasploitable 2 target to identify exposed services and gather information that could be used during later stages of the assessment.

## Basic Scan

An example scan used during the lab was:

nmap -sC -Pn -sV -oN scan.txt 10.xx.xx.xx

The options used here are:

| Option | Meaning |
|---|---|
| `-sC` | Run Nmap's default NSE scripts for additional enumeration. |
| `-Pn` | Skip host discovery and treat the target as online. Useful when ICMP/ping probes are blocked or ignored. |
| `-sV` | Perform service/version detection to identify the software and versions running on discovered ports. |
| `-oN scan.txt` | Save the results in Nmap's normal, human-readable output format. |

The combination of `-sC`, `-sV`, and `-Pn` allows a scan to go beyond simply identifying open ports and gather additional information about the services running on the target.

## TCP Scan Types

### TCP Connect Scan — `-sT`

A TCP connect scan completes a normal TCP connection with the target.

The basic handshake is:

```text
SYN → SYN/ACK → ACK
```

Because the connection is completed, this scan is more easily logged by the target than a SYN scan.

### SYN Scan — `-sS`

A SYN scan sends the initial SYN packet but does not normally complete the TCP handshake.

```bash
SYN → SYN/ACK
     ↳ RST
```

For an open port, the target normally responds with SYN/ACK, after which Nmap sends RST rather than completing the connection.

Because the full TCP connection is not established, this is sometimes referred to as a "half-open" scan.

### FIN Scan — `-sF`

A FIN scan sends a TCP packet with the FIN flag rather than initiating a normal TCP connection.

Its interpretation differs from a normal TCP connection:

`RST` response generally indicates a closed port.
No response generally indicates open|filtered, because a firewall may also silently drop the packet.

Therefore, a FIN scan cannot always distinguish an open port from one filtered by a firewall.

### UDP Scan — `-sU`

UDP does not establish a TCP-style connection, so a UDP scan sends UDP packets to the target ports.

A common interpretation is:

- An ICMP "port unreachable" response indicates the port is closed.
- No response may indicate open|filtered.

UDP scanning can therefore be slower and less definitive than TCP scanning.

## Scanning Ports

By default, Nmap does not necessarily scan every TCP port. The -p option can be used to specify which ports should be scanned.

Scan all ports:

```bash
nmap -p- 10.x.x.x
```

The `-p-` syntax tells Nmap to scan ports 1–65535.

### Scan a specific port

```bash
nmap -p 80 10.x.x.x
```

This limits the scan to port 80.

## Common Lab/CTF Scan

A useful starting point for an intentionally vulnerable lab target is:

```bash
nmap -sC -sV -p- -Pn 10.x.x.x
```

This combines:

- default NSE scripts (`-sC`)
- service/version detection (`-sV`)
- all TCP ports (`-p-`)
- skip host discovery (`-Pn`)

The purpose is to obtain a broad initial picture of the target's exposed TCP services before moving on to vulnerability research and exploitation.
