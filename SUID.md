# SUID

SUID, Set User ID, is a special permission bit flag that applies to files and directories to provide elevated access as the file owner during execution. In a permission string, the SUID bit is represented as an "s" and applied to the user permission field. Below is an example of a directory where the file contents can be executed with the permissions of the directory owner.
```
drwsrwxr--  2  kayem  kayem    4096  Jul 22  19:45  kayem
```
You can apply SUID bits to a file or directory with octal or symbolic representations. Using octal representation looks like this:
```
chmod 4774 kayem
```
The 4 represents the SUID bit and the following numbers are permissions applied to each consecutive permission set.

To set the SUID bit with symbolic representation you can use the following command:
```
chmod u+s kayem
```
This command adds the SUID bit to the user permission set.

# SGID

SGID, Set Group ID, is another special permission bit that provides elevated access as the group owner during execution. The SGID bit is represented as an "s" or "S" in the group set of a permission string, where the "s" represents execute permissions and the "S" represents no execute permissions. You can apply SGID bits to a file or directory with octal or symbolic representations. Using octal representation looks like this:
```
chmod 2774 kayem
```
With the `ls -l` command, the permissions applied to the kayem directory are:
```
drwxrwsr-- 2 kayem kayem   4096 Jul 22 19:45 kayem
```
If we want to remove execute permissions for the group we can use the following command:
```
chmod 2764 kayem
```
The permissions applied to the directory are now:
```
drwxrwSr-- 2 kayem kayem   4096 Jul 22 20:12 wassup
```
To set the SGID bit with symbolic representation, you can use the following command:
```
chmod g+s kayem
```
To remove the SGID bit with symbolic representation, you can use the following command:
```
chmod g-s kayem
```

# Practical

Although you can use automated tools like Linpeas (https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS) to hunt for potentially vulnerable SUID bit set programs, it is important to understand how you can manually hunt for them. The following command uses the 'find' program to search the root directory of the file system for all files with the SUID bit set whilst redirecting all standard errors to the /dev/null directory.
```
find / -perm -u=s -type f 2>/dev/null
```
You can complement this command with GTFOBins (https://gtfobins.github.io/), a library of binaries that can be used against command-line utilities to elevate privileges. Running this command on the target system revealed the base64 binary stored at `/usr/bin/base64`, with the help of GTFOBins, the following command can be used to read the contents of the `/etc/shadow` file.
```
LFILE=/etc/shadow
base64 "$LFILE" | base64 --decode
```
After executing this command, you should have the hashes for all users on the system and can crack them with Hydra, Hashcat, JohnTheRipper, or perform a Pass-The-Hash attack.
