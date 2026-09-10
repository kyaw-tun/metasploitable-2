# Summary

## Course Overview

This documentation is based on the Red Team Essentials course by WYWM (Greenbeam).

The course introduced the basic workflow and techniques used during a red team essentials course work, starting from reconnaissance and enumeration, followed by vulnerability research, brute-force attacks, exploitation, initial access, post-exploitation, and privilege escalation.

The practical exercises were performed against Metasploitable 2, an intentionally vulnerable virtual machine, using Kali Linux as the attacker machine.

## Assessment Flow

The course explored two main paths to gaining access to and escalating privileges:

```text
Red Team Essentials
        │
        ├── Initial access / exploitation path 1
        │       └── Metasploit
        │              └── Root
        │
        └── Initial access / exploitation path 2
                └── Tomcat
                       └── Initial access
                              └── Enumeration
                                     └── Privilege escalation
                                            └── Root
```

Along the way, the course covered:

- Network and service enumeration with Nmap
- Searching for known exploits with SearchSploit
- Researching vulnerabilities through Exploit-DB
- Brute-forcing Tomcat credentials with Hydra
- Exploitation with Metasploit
- Payload generation with MSFvenom
- Linux enumeration with LinPEAS and LinEnum
- Identifying privilege-escalation opportunities
- Using GTFOBins to research potential abuse of existing Linux binaries

## Lab Setup

The main setup issue I encountered was getting Metasploitable 2 running under QEMU/virt-manager, as the original setup did not directly support the virtualization environment I was using.

I used ChatGPT to help troubleshoot the setup and adapt the installation process to my environment.

After adapting the installation, I was able to run the target machine alongside Kali Linux and continue with the assessment.

## Current Status

Although, I have finished documenting the Red Team essentials course of exploiting the Metasploitable 2 virtual machine,  I may continue experimenting with the Metasploitable 2 environment and add additional techniques or discoveries to the repository later.
