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
