# Nmap Scan Results

## Target

- Target: Metasploitable 2
- IP address: `192.168.122.68`
- Scan date: August 14, 2026
- Nmap version: 7.99

## Command Used

```bash
nmap -sC -Pn -sV -oN scan.txt 192.168.122.68
```

The scan identified 23 open TCP ports, while 977 ports were reported as closed.

## Discovered Services

|Port|	Service	|Version / Information|
|---|---|---|
|`21`|	FTP|	vsftpd 2.3.4; anonymous login allowed|
|`22`|	SSH|	OpenSSH 4.7p1|
|`23`|	Telnet|	Linux telnetd|
|`25`|	SMTP	|Postfix; SSLv2 supported|
|`53`|	DNS|	ISC BIND 9.4.2|
|`80`|	HTTP|	Apache 2.2.8|
|`111`|	RPC|	rpcbind|
|`139/445`|	SMB|	Samba 3.0.20|
|`512`	|exec|	netkit-rsh rexecd|
|`513`	|login|	Remote login service|
|`514`	|tcpwrapped|	Service detected as tcpwrapped|
|`1099`|	Java RMI|	GNU Classpath grmiregistry|
|`1524`|	Bindshell|	Metasploitable root shell|
|`2049`|	NFS|	NFS v2–4|
|`2121`|	FTP|	ProFTPD 1.3.1|
|`3306`|	MySQL|	MySQL 5.0.51a|
|`5432`|	PostgreSQL|	PostgreSQL 8.3.x|
|`5900`|	VNC|	VNC protocol 3.3|
|`6000`|	X11|	Access denied|
|`6667`|	IRC|	UnrealIRCd 3.2.8.1|
|`8009`|	AJP|	Apache JServ|
|`8180`|	HTTP|	Apache Tomcat 5.5|

## Notable Findings

Several results stood out as potential avenues for further investigation.

### FTP

Port `21` was running vsftpd 2.3.4, and Nmap's default scripts reported that anonymous FTP login was allowed.

The combination of an old service version and anonymous access made this service worth investigating further.

### SMB

Ports `139` and `445` were running Samba 3.0.20.

Nmap's NSE scripts also provided additional information about the system:

- Hostname: `metasploitable`
- FQDN: `metasploitable.localdomain`
- Workgroup: `WORKGROUP`
- SMB message signing: disabled

The detected Samba version was particularly interesting for subsequent vulnerability research.

### UnrealIRCd

Port `6667` was running UnrealIRCd 3.2.8.1.

The service was identified during the initial Nmap scan and was later investigated and exploited as part of the course. This made the Nmap version-detection result particularly useful, as the identified software and version provided a starting point for researching known vulnerabilities.

The exploitation process is documented later in the coursework.

### Tomcat

Port `8180` was running Apache Tomcat 5.5, with port `8009` also exposing the Apache JServ (AJP) service.

This provided another potential avenue for investigation, particularly because the service version was clearly identified by Nmap.

### Other Exposed Services

The scan also revealed several other services that could potentially expand the attack surface, including Telnet, NFS, VNC, MySQL, PostgreSQL, Java RMI, IRC, and remote shell services.

Because this was an intentionally vulnerable Metasploitable 2 environment, the large number of exposed and outdated services provided multiple opportunities for learning and experimentation.

### Next Step

The Nmap scan provided the initial service inventory for the target. The next step was to investigate the discovered software versions for known vulnerabilities and available exploits.

This led to the vulnerability research documented in the SearchSploit section.
