# Finding an Opportunity for Escalation

In this lesson, we went through the information gathered by LinPEAS and LinEnum and looked for anything that could provide an opportunity for privilege escalation.

LinPEAS provides a lot of information in its output, and the colors make it easier to identify different types of information. It also provides a legend explaining what the different colors represent. LinEnum provides similar information in a more plain-text format.

The purpose of this part was mainly to go through the enumeration results and understand what information could be useful for finding an escalation opportunity.

While going through the enumeration results, I took this opportunity to learn more about Linux file permissions. I had seen permission formats such as `drwxr-xr-x` and `rwxr--r--` before, but I wasn't fully comfortable interpreting them. I started looking into permissions during this lesson, and continued learning about them afterwards.

## Understanding File Permissions

The first character indicates the file type. A `-` represents a regular file, while `d` represents a directory.

The remaining nine characters are divided into three groups:

```text
-rwxr-xr-x
 │   │   │
 │   │   └── Others
 │   └────── Group
 └────────── Owner
```

Each group has three permissions:

- `r` — read
- `w` — write
- `x` — execute

For example, `-rwxr-xr-x` means:

- Owner: read, write, execute
- Group: read, execute
- Others: read, execute

## SUID Permission

The part I wasn't familiar with was seeing an `s` in place of the owner's `x`, such as:

```text
-rwsr-xr-x
```

The `s` in the owner's execute position indicates that the SUID (Set User ID) permission is set.

When a file has SUID set, executing that file causes it to run with the effective privileges of its owner. If the owner is `root`, the program runs with effective `root` privileges.

At the time of this lesson, I wouldn't have recognized `-rwsr-xr-x` as a SUID file because I was mainly familiar with the more common `rwx` permissions. Learning about the normal permission format first made it easier to understand what the `s` represented and why a SUID file owned by `root` could be worth investigating as a potential privilege-escalation opportunity.
