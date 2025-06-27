replaced NTLM
Kerberos (U:88, T:389)
Kerberos
U:88 - Kerberos services
T:389 - LDAP services

- **Kerberos Protocol**: Enables mutual authentication between clients and services.
- **Authentication Process**: Verifies users and services before session initiation.
- **Ticket System**: Utilizes shared key encryption for security. Users receive tickets granting access to network resources, and servers use these tickets to confirm user identity.
- **Domain Dependency**: All elements in a Kerberos transaction must be within the same domain, often Active Directory.

```mermaid
graph TD
subgraph Key Distribution Center - KDC
AuthenticationServer[<b>Authentication Server<br>AS</b><br>Check your passwd<br>Make sure you have account on that network<br> .]
TicketGrantingServer[<b>Ticket Granting Server<br>TGT</b><br>Issue ticket to acces file servers, printers, etc.]
end

```

# Kerberos Authentication

## explained in details

**Kerberos principal** - a unique entity (a user or service) to which it can assign a ticket. A principal consists of the following components:
_USER_/_INSTANCE_@_REALM_

- primary: The first part of the principal. For users, this is usually the same as the user name.
- instance _(optional)_: Additional information characterizing the _primary_. This string is separated from the _primary_ by a `/`. `tux@example.org` and `tux/admin@example.org` can both exist on the same Kerberos system and are treated as different principals.
- realm: Specifies the Kerberos realm. Normally, your realm is your domain name in uppercase letters.

[Kerberos deep dive with message content and keys](https://www.youtube.com/watch?v=5N242XcKAsM)
[Sunny Classroom](https://www.youtube.com/watch?v=_44CHD3Vx-0&t=2s)
[Computerphile (YT) - Kerberos explained](https://www.youtube.com/watch?v=qW361k3-BtU)
[Network authentication with Kerberos - SUSE docs](https://documentation.suse.com/pl-pl/sles/15-SP5/html/SLES-all/cha-security-kerberos.html)

![alt text](https://upload.wikimedia.org/wikipedia/commons/6/68/Kerberos_protocol.svg)

```mermaid
sequenceDiagram
    participant Client as Client<br><br>Knows:<br>Ku (user key)
    box Key Distribution Center (KDC)<br>Domain Controller
        participant AS as Authentication Server (AS)<br><br>Knows:<br>Ku (user key)<br>Ktgs (TGS key, NTLM hash of the krbtgt account)
        participant TGS as Ticket Granting Server (TGS)<br><br>Knows:<br>Ktgs (TGS key)<br>Kservice (service key)
    end
    participant Server as FileServer<br>(Service)<br><br>Knows:<br>Kservice (service key)<br>(password hash of<br>the service account)

    autonumber

    %% AS-REQ (Authentication Service Request)
    Note left of Client: Create Ku<br>based on UserID
    Client-)AS: AS-REQ (unencrypted)<br><br>UserID, ServiceID (FileServer),<br>UserIP, Lifetime for TGT<br>Timestamp, RandNum to prevent replay attacks<br>...<br>timestamp and other data<br>may be encrypted with Ku
    Note left of AS: Retrieve User Passwd from<br>C:/Windows/NTDS/NTDS.dit<br>database based on UserID<br>Create Ku<br>Decrypt request<br>Verify User Identity
    Note left of AS: Generate K_tgs_ses<br>Ticket Granting Server Session Key
    Note left of AS: Create TGT

    %% AS-REP (Authentication Service Reply)
    AS-)Client: AS-REP<br><br>TGS Name/ID, Timestamp, Lifetime, Enc(K_tgs_ses, Ku),<br>+<br>Enc(TGT(K_tgs_ses),Ktgs)
    Note over Client, AS:TGT={UserID, TGS ID, Timestamp, Lifetime, Enc(K_tgs_ses, Ktgs)}
    Note right of Client: Receive Enc(TGT(K_tgs_ses),Ku)<br>Decrypts<br>-><br>K_tgs_ses  
    Note left of Client: Receive<br>K_tgs_ses  
    Note right of Client: Authenticator<br>=<br>UserID+Timestamp

    %% TGS-REQ (Ticket Granting Service Request)

    Client-)TGS: TGS-REQ<br>Unencrypted(ServiceName/ID, Lifetime for TGS)<br>+<br>Enc(Authenticator,K_tgs_ses)<br>+<br>Enc(TGT(K_tgs_ses),Ktgs)
    %%TGT, Service SPN, 
    Note left of TGS: Decrypt Enc(TGT(K_tgs_ses),Ktgs)<br>-><br>K_tgs_ses
    Note left of TGS: Decrypt Enc(Authenticator,K_tgs_ses)<br>-><br>Authenticator

    %% TGS-REP (Ticket Granting Service Reply)
    Note left of TGS: Generate K_serv_ses<br>(Service Session Key)
    Note left of TGS: Create ST
    TGS-)Client: TGS-REP<br><br>Service Name/ID, Timestamp, Lifetime, Enc(K_serv_ses, K_tgs_ses),<br>+<br>Enc(ST(K_serv_ses),Kservice)<br>
    Note over Client, TGS:ST={UserID, ServiceID, Timestamp, Lifetime, Enc(K_tgs_ses, K_tgs)}
    Note right of Client: Receive Enc(K_serv_ses, K_tgs_ses)<br> Decrypts<br>-><br>K_serv_ses
    Note left of Client: Receive<br>K_serv_ses

    %% AP-REQ (Application Request)
    
    Note right of Client: Authenticator<br>=<br>UserID+Timestamp
    Client-)Server: AP-REQ<br><br>Enc(Authenticator, K_serv_ses)<br>+<br>Enc(ST(K_serv_ses),Kservice)
    Note left of Server: Receive Enc(ST(K_serv_ses),Kservice)<br>Decrypt <br>-><br>K_serv_ses
    Note left of Server: Receive Enc(Authenticator, K_serv_ses)<br>Decrypt<br>-><br>Authenticator
    Note left of Server: Compare<br>Authenticator and Service Ticket

    %% AP-REP (Application Reply - Optional)
    Server-)Client: AP-REP<br>Enc(Authenticator, K_serv_ses<br><br>Authenticator = ServiceName/ID + Timestamp

```