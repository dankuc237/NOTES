#TODO
Selfsigned Certificate https://www.youtube.com/watch?v=oplVr3Ha174&list=PLSNNzog5eyduzyJ8_6Je-tYOgMHvo344c&index=3
PKI - Third Party Trust - https://www.youtube.com/watch?v=LPxeYtMDxl0&list=PLSNNzog5eyduzyJ8_6Je-tYOgMHvo344c&index=4
https://www.youtube.com/watch?v=33VYnE7Bzpk

# TLS
```mermaid
sequenceDiagram
    participant Client as Browser (Client)
    participant Server as Web Server
	
	Note over Server: Got  SSL/TLS Certificate<br>issued beforehand by CA
	
	rect grey
    Note over Client, Server: TLS key exchange + X.509 certificate
    Client->>Server: Browser request Certificate<br>Client Hello - (TLS version,<br>supported cipher suites...)
    
    Server->>Client: Server Hello<br>(TLS Certificate<br>+ selected cipher suite<br>+ server public key)
    
	Note over Client: Cert Verification<br>with Certificate Authority

	Note over Client, Server: Client Key Exchange
	Note over Client: Generate Shared Key<br>Encrypt with server's public key
	
	Client ->> Server: Client sends the encrypted Shared Key
	Client ->> Server: Finish message encrpted with Shared Key
    
	Note over Server: Decrypt Client Key<br>with server`s private key
	Note over Server: Only server owns<br>the matching private key.
	
	Note over Client, Server:Both got Shared Key
	Client ->> Server: "Finish" message encrpted with Shared Key
	Server ->> Client: "Finished" message encrpted with Shared Key
	
	end
	Note over Client, Server: Both sides got Shared Session Key<br> Symetric Key Encryption
```