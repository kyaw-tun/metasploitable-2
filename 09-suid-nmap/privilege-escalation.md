# Privilege Escalation with SUID

In the previous lesson, we went through the output from LinPEAS and LinEnum to look for possible privilege-escalation opportunities. LinPEAS uses different colors to highlight information that may be worth investigating.

When running LinPeas, it is explained that the orange-highlighted results have a high chance of being an attack vector, while red-highlighted results are also worth checking. There are other colors in the output as well, each with its own meaning according to the LinPEAS legend.

While going through the results, I looked for the orange and red highlighted information. There were several results that could be investigated further, and the SUID-enabled Nmap binary was one of them.

## GTFOBins

Another useful resource encountered here was GTFOBins, a reference for Unix binaries that can be used in ways that may lead to privilege escalation, escaping restricted shells, and other security-related techniques.

Nmap has an entry on GTFOBins showing that certain versions can be used to obtain a shell when the binary has elevated privileges.

## Nmap Interactive Mode

The target is running a version of Nmap that supports the interactive mode:

```bash
nmap --interactive
```

This opens the Nmap interactive prompt.

From there, the `!` command can be used to execute a shell command:

```bash
!sh
```

Because the Nmap binary has SUID set and is owned by `root`, the shell is executed with `root` privileges.

This demonstrates how a SUID-enabled binary can become a privilege-escalation opportunity when the program provides a way to execute commands or spawn a shell.

The interactive mode used in this lesson is specific to older versions of Nmap. Modern versions of Nmap no longer include this interactive mode.
