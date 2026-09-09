# Red Team Essentials — Metasploitable 2

This repository documents my hands-on learning from the Red Team Essentials course.

The course uses Metasploitable 2 as an intentionally vulnerable target and introduces core red-team techniques including
network reconnaissance, vulnerability research, exploitation, post-exploitation enumeration, and privilege escalation.

Each section corresponds to a lesson from the course and contains my notes, commands, explanations, practical exercises, and supporting evidence.

The repository is intended as a learning reference rather than a standalone penetration-testing project.

## Course Progression

| # | Topic | Skill |
|---|---|---|
| 00 | Lab Setup | QEMU/KVM/libvirt deployment |
| 01 | Nmap | Network/service enumeration |
| 02 | SearchSploit | Local exploit research |
| 03 | Exploit-DB | Public exploit research |
| 04 | Metasploit | Exploitation framework |
| 05 | Tomcat | Web server exploitation |
| 06 | Enumeration Script | Post-exploitation enumeration |
| 07 | Service Account | Account/environment enumeration |
| 08 | Escalation Opportunity | Identifying attack paths |
| 09 | SUID + Nmap | Privilege escalation |

## A Glimpse

The course follows a practical progression from discovering exposed services to gaining access and escalating privileges on the target.

Some of the techniques explored include:

- Reconnaissance — discovering open ports and running services
- Vulnerability Research — connecting discovered services to known exploits
- Exploitation — using Metasploit and other techniques to gain access
- Post-Exploitation — enumerating the compromised system
- Privilege Escalation — identifying and abusing opportunities to reach root

The detailed notes and evidence for each lesson can be found in the corresponding sections above.

## Lab Environment

- Attacker: Kali Linux
- Target: Metasploitable 2
- Virtualization: QEMU/KVM + libvirt

For more details about the setup and references, see [references](/references.md).

## Notes

The course material documented here represents what I have learned so far. Metasploitable 2 contains many other vulnerabilities and attack paths that are not covered by this course, so I may explore them independently in the future.

## Related

This course material was followed by a separate Red Team Capstone, where I applied the techniques learned throughout the course against a different intentionally vulnerable environment.

→ [Red Team Capstone](https://github.com/kyaw-tun/red-team-capstone)
