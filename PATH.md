# Path

PATH is an environmental variable that contains directories where to find executable files. Whenever a binary such as "ls" is used, the shell refers to the PATH and searches each directory in the order they appear to retrieve the absolute path of the binary. You can view your PATH by running
```
echo $PATH
```
Adding a directory to a PATH is beneficial as an executable file can be run from anywhere on the file system without specifying the absolute path. You can temporarily add a directory to the PATH with:
```
export PATH=[/DIRECTORY]:$PATH
```
Or permanently adding the directory to the shell file by first checking the shell script running on your terminal and then editing the shell script.
```
echo $0
```
## Exploiting PATH

If there is a "." in the PATH variable, the user can execute binaries from their current directory.

When attempting to exploit a binary, ensure it does not appear in a prior PATH directory as it will be executed instead of your malicious binary.

First, find a writable directory that can be added to the PATH variable. The command below will traverse the directory until it is 3 levels deep.
```
find / -type d -writable -maxdepth 3 2>/dev/null
```

# Practical

I begin by enumerating the file system, I locate two files in the /home/murdoch directory `test` and `thm.py`. After running `test` the shell returns `thm: not found` indicating that the shell is looking for the binary. 

By running `ls -l` in the current /home/murdoch directory, the `test` executable has 4777 permissions and is owned by root meaning that `thm` will be executed as root. Therefore, we can create a new file called thm and add a `/bin/bash` payload that opens a new terminal instance into this file.
```
echo "/bin/bash" > thm
```
We then need to add execute permissions to the file so that it can be executed by `test`
```
chmod +x thm
```
Finally, we run the `test` executable and should be the root user.
