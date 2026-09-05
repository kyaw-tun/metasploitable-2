# Hosting Enumeration Script

In this lesson, it was explained how Python can be used to quickly host a simple HTTP server. This can then be used to access files from another machine.

## Hosting a Web Server with Python

### Python 2

With Python 2, the `SimpleHTTPServer` module can be used:

```bash
python2.7 -m SimpleHTTPServer 1337
```

This starts an HTTP server on port `1337`.

The server can be tested from another terminal with:

```bash
curl localhost:1337
```

### Python 3

For Python 3, the `http.server` module is used instead:

```bash
python3 -m http.server
```

By default, this starts the server on port `8000`.

The server can be tested from another terminal with:

```bash
curl localhost:8000
```

## Downloading Enumeration Scripts

The lesson also introduced two Linux enumeration scripts, LinEnum and LinPEAS.

### LinEnum

LinEnum can be downloaded with:

```bash
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh
```

### LinPEAS

LinPEAS can be downloaded with:

```bash
wget https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh
```

## What Was New

One thing that was new in this lesson for me was that Python can be used to quickly host a web server. Before this lesson, I mostly associated hosting files over HTTP with using a dedicated web server such as Apache. I didn't realize that Python itself could provide a simple HTTP server without needing to set up something like Apache.

The use of `curl` and `wget` was already familiar, but this connected them with a simple way of making files available from another machine.
