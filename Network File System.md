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

Older NFS versions such as `NFSv2` and `NFSv3` lack robust authentication and authorisation mechanisms and can be easily bypassed to gain unauthorised access to shares on the file system or privileged access.

`NFSv2` authenticates users with UID, GID and group memberships. Therefore, it is possible to access an NFS mount by impersonating the UID of the mount owner. You can accomplish this by mounting a directory to the target using `NFSv3`.
```
mount -o vers=3 <IP_ADDRESS>:/<SHARE> <MOUNT_POINT>
```
`NFSv3` allows you to view the UIDs of mount owners, therefore once you have retrieved the UID, you can remount to the NFS export with the spoofed UID and gain access.  
