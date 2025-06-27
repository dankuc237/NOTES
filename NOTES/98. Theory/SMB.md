
Simple Note:

- **Port 139**: Originally, SMB operated on top of NetBIOS using port 139. NetBIOS is an older transport layer enabling Windows computers on the same network to communicate.
- **Port 445**: Used for newer SMB versions (after Windows 2000) over TCP, allowing SMB to function over the internet.
- **Windows Automatic Resources**: 
  - **ADMIN$**: Provides access to the system directory (usually `C:\Windows`), simplifying system administration tasks. **Example Usage:** Enables remote management of files and folders in the system directory, remote software installations, etc.
  - **IPC$ (Inter-Process Communication)**: Facilitates communication between processes on remote computers. **Example Usage:** Allows remote execution of commands and other operations requiring inter-process communication.
  - **C$**: Grants access to the C drive on remote computers, aiding in remote management tasks. **Example Usage:** Enables remote browsing, management, and manipulation of files and folders on the C drive of a remote computer.
- **SMB Protocol**: Enables communication between clients and servers in Microsoft networks, governing file-sharing and remote administration.
- **Exploits**: 
  - Offline cracking of NTLM Challenges to recover passwords.
  - Man-in-the-middle attack on SMB authentication to gain access to servers.