# SearchSploit

SearchSploit is a command-line tool for searching the local Exploit-DB archive for publicly available exploits.

During the Red Team Essentials course, I used SearchSploit after identifying services and versions with Nmap. The search results helped identify known exploits that could be investigated further.

## Updating the Database

The local Exploit-DB database can be updated with:

```bash
searchsploit --update
```

This updates the local exploit database used by SearchSploit.

## Searching for Exploits

SearchSploit can be used to search for exploits by software name, version, operating system, or other keywords.

For example:

```bash
searchsploit debian linux kernel 2.6.32 priv esc
```

A more specific search can also be performed:

```bash
searchsploit linux kernel 2.6.32 priv esc
```

During the course, I also searched for exploits affecting the UnrealIRCd service identified during the Nmap scan:

```bash
searchsploit unrealircd
```

The search returned several results, including:

```bash
UnrealIRCd 3.2.8.1 - Backdoor Command Execution (Metasploit) | linux/remote/16922.rb
```

The Nmap scan had identified UnrealIRCd 3.2.8.1 running on port `6667`, so this exploit was relevant to the service and version discovered during reconnaissance.

## Locating an Exploit

After identifying EDB-ID 16922, the `-p` option can be used to locate the exploit in the local Exploit-DB archive:

```bash
searchsploit -p 16922.rb
```

The command returned the local path:

```bash
/usr/share/exploitdb/exploits/linux/remote/16922.rb
```

It also identified the exploit as a Ruby script and associated it with CVE-2010-2075.

## Copying an Exploit

The `-m` option can be used to mirror an exploit from the local Exploit-DB archive into the current working directory:

```bash
searchsploit -m 16922.rb
```

This copies the `.rb` exploit file to the current working directory, where it can then be examined or used during the subsequent exploitation stage.

## Workflow

The SearchSploit workflow followed the information gathered during reconnaissance:

```text
Nmap
  ↓
UnrealIRCd 3.2.8.1 identified on port 6667
  ↓
searchsploit unrealircd
  ↓
EDB-ID 16922 identified
  ↓
searchsploit -p 16922.rb
  ↓
Exploit located
  ↓
searchsploit -m 16922.rb
  ↓
Local copy obtained
```

The identified exploit was later used as part of the UnrealIRCd exploitation exercise documented in the following sections.
