# Network File System

Network File System (NFS) is a client-server file-sharing protocol used across Unix systems. NFS enables remote clients to access resources on a network's file system and mount shares, a process by which remote network resources, such as a directory, are attached to a mount point on the file system.

# Enumeration

Exported directories are directories made available by the NFS server to NFS clients for access and mounting. With the following `showmount` command, you can view available exported directories on a particular NFS server.
```
showmount -e <IP_ADDRESS>
```
Alternatively, you can view shares the NFS server is configured to export and share with clients.
```
cat /etc/exports
```
Shares that are currently mounted on the file system are stored.
```
cat /proc/mounts
```
