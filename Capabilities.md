# Capabilities

Capabilities are a per-thread attribute that ivides the privileges assosciated with 'root' into distinct units, known as capabilities. By providing programs with capabilities, you reduce the damage that the program can do compared to the same program running with full root privileges. The following lists Linux capabilities:

- CAP_CHOWN: Makes arbitrary changes to file UIDs and GIDs.
- CAP_DAC_OVERRIDE: Bypasses file read, write, and execute permissions check.
- CAP_NET_ADMIN: Performs network-related operations.
- CAP_NET_RAW: Uses RAW and PACKET sockets.
- CAP_SETGID: Manipulates GIDs
- CAP_SETFCAP: Sets abitrary capabilities on a file.
- CAP_SETUID: Manipulates UIDs
- CAP_SYS_ADMIN: Performs a rane of system administration operations.

Each thread has the following capability sets:

- Permitted
- Inheritable
- Effective
- Bounding
- Ambient
