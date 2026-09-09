# Tomcat Web Server

This section covers the Tomcat web server running on the Metasploitable 2 target and demonstrates how weak authentication can lead to access to the Tomcat Manager interface.

## Accessing the Tomcat Web Server

The Tomcat web server is accessible on port `6667` of the target machine.

The web interface can be accessed through a browser using the target's IP address and port.

```bash
http://192.168.122.68:6667
```

The Tomcat web page confirms that the service is running.

## Tomcat Manager

From the Tomcat page, I went to the `/manager` directory.

The Manager application was password protected, so I needed to find a way to get valid credentials.

There are two approaches to obtaining valid credentials:

- Trying known/default credentials manually.
- Using Hydra to automate credential testing against the authentication service.

### Finding the default credentials online

Searching online for the tomcat's default credentials is very convenient. I just typed in:

```text
tomcat default credentials
```

on Google and there are multiple links showing the results. And I chose the first link which is a [GitHub page](https://gist.github.com/0xRar/70aae102af56495b7be51486d363c4bd) on default passwords.

I also saved a screenshot of the result in the `evidence/screenshots` directory.

### Brute-Forcing with Hydra

Another way to find the credentials is to use Hydra, a tool for brute-forcing login credentials against different network services.

For this exercise, I used Hydra against the Tomcat Manager login.

First, I made sure Hydra and SecLists were installed:

```bash
sudo apt update && sudo apt install seclists hydra
```

I then used Hydra with a known username and password:

```bash
hydra -l username -p password -s 6667 -f 192.168.122.68 http-get /manager/html
```

Here:

- -l specifies a single username.
- -p specifies a single password.
- -s specifies the port where the service is running.
- -f tells Hydra to stop after finding a valid login.
- `192.168.122.68` is the target IP address.
- `http-get /manager/html` specifies the HTTP service and the path to the login page.

You can also use username and password lists instead:

```bash
hydra -L username-list -P password-list -s 6667 -f 192.168.122.68 http-get /manager/html
```

In this case:

- -L uses a list of usernames.
- -P uses a list of passwords.

The important part here is that Hydra needs to know where the login is and which credentials it should try.

This gave me two different ways to approach a protected Tomcat Manager login: checking for default credentials or brute-forcing the credentials with Hydra.
