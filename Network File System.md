# Network File System

Network File System (NFS) is a client-server file-sharing protocol used across Unix systems. NFS enables remote clients to access resources on a network's file system and mount shares, a process by which remote network resources, such as a directory, are attached to a mount point on the file system.
```
sudo mount -t nfs <IP_ADDRESS>:/<SHARE> <MOUNT_POINT>
```
# Enumeration

Exported directories are directories made available by the NFS server to NFS clients for access and mounting. With the following `showmount` command, you can view available exported directories on a particular NFS server.
```
showmount -e <IP_ADDRESS>
```
Alternatively, you can view shares the NFS server is configured to export and share with clients.
```
cat /etc/exports
```
Shares that are currently mounted on the file system are stored. If no proper authentication or authorisation mechanisms are used, these shares can be exploited to escalate privileges or to exfiltrate sensitive information. 
```
cat /proc/mounts
```

# Exploiting NFS
### Bypassing UID permissions

Older NFS versions such as `NFSv2` and `NFSv3` lack robust authentication and authorisation mechanisms and can be easily bypassed to gain unauthorised access to shares on the file system or privileged access.

`NFSv2` authenticates users with UID, GID and group memberships. Therefore, it is possible to access an NFS mount by impersonating the UID of the mount owner. You can accomplish this by mounting a directory to the target using `NFSv3`.
```
mount -o vers=3 <IP_ADDRESS>:/<SHARE> <MOUNT_POINT>
```
```
ls -n
```
With `NFSv3`, you can view the UIDs of mount owners; therefore, once you have retrieved the UID, you can remount to the NFS export with the UID of a particular mount owner and gain access.  
```
sudo useradd -u <UID> <USERNAME>
```
```
su <USERNAME>
```
### Exploiting configurations

Root squashing is an NFS feature that restricts the privileges of a root user by assuming all root users as low-privileged users. Root squashing is enabled by default; however, some networks may require remote root access and specify the `no_root_access` option in the NFS server's configuration file.

With this option configured, you can create a directory on your local machine as the root user, mount it to the file system, and you will notice that the mounted directory will be owned by the root user on the file system.

You can then create a "/bin/bash" payload file with the SUID bit enabled so that when run on the NFS file system, a root shell will be spawned on it.
