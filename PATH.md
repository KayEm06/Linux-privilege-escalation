# Path

PATH is an environmental variable that contains directories where to find executable files. Whenever a binary such as "ls" is used, the shell refers to the PATH and searches each directory in the order they appear to retrieve the absolute path of the binary. You can return the details of the PATH variable with,
```
echo $PATH
```
Adding a directory to a PATH is beneficial as an executable file can be run from anywhere on the file system without specifying the absolute path. You can temporarily add a directory to the PATH with:
```
export PATH=[/DIRECTORY]:$PATH
```
Alternatively, you can permanently add the directory to the shell file.

## Exploiting PATH

PATH variables representing "." enable users to execute binaries from their current directory.

If you don't have permission to modify the PATH variable, place the cloned program in a directory that appears before the directory with the legitimate program in the PATH variable.

Find writable directories that can be added to the PATH variable with the following command that traverses through three layers of each directory.
```
find / -type d -writable -maxdepth 3 2>/dev/null
```

# Practical

I locate two files in the /home/murdoch directory `test` and `thm.py`. After running `test`, the shell returns `thm: not found`, indicating that the shell is looking for a `thm` binary. 

Since the `test` file has 4777 permissions and is owned by the root user, a file that replicates `thm` can be added to the PATH variable with the `/bin/bash` payload.
```
echo "/bin/bash" > thm
```
Add execute permissions to the file to enable execution.
```
chmod +x thm
```
Finally, we run the `test` executable to run a bash instance as the root user.
```
./test
```
