#### FTP (File Transfer Protocol)
It is a protocol for out-of-band method.
- controlling connection: port 21, persistent connection type
    - user authentication, password, commands such as cd, put and get
- data connection: port 20, non-persistent connection type
    - Transfer actual files

#### E-mail
components
- User agent (mailer)
    - Outlook, Thunder bird, etc...
- Mail server
    - Post fix
- Protocol
    - SMTP (Simple Mail Transfer Protocol)
    - Mail access protocols
        - POP3, IMAP
Use SMTP in communications between a sending user agent and a server, or among servers.
Use mail access protocols between a mail server and an end user agent.

##### SMTP
Transfer message from the sending mail server to the receiving mail server
- Use 7 bits ASCII code as a data format
1. Client SMTP connects to port 25 of server SMTP using TCP
2. Handshake **in the application layer**
    (a) The client lets the server know the sender address, and the server responds with sending grant.
    (b) The client lets the server know the destination address, and the server notifies if the destination address actually exists. *In fact, mailer daemons occur due to this!*
3. The client notifies the start of data transport, and actually starts it soon after it receives the server's response.
4. Once transmission finishes, cut the connection
- TCP connection is persistent connection type.
    If there are multiple mails to one server, they are transported using only one connection.

##### Mail access protocol
The end user agent retrieves mail from the mail server.
- POP3 (Post Office Protocol ver.3)
    - Manages mails on client side
    - It is general to delete mail on server side after a designated term after finishing fetching the mail.
- IMAP (Internet Mail Access Protocol)
    - Manages mails on server side
    - Mail folder distribution and syncing sent mail between different terminals. (*It allows mails to have additional information such as folder, flag, etc.*)
- Recently, HTTP based web-based E-mails (Gmail) are mainly used. (It still uses SMTP for server-to-server communication. **User access uses HTTP**)

#### DNS
DNS (Domain Name System)
- Hierarchical distributed database composed of name servers
- The application layer protocol that handles communication between hosts and name servers
- *Most system failures are due to DNS failures.*
##### Provided services
- Translate domain names into IP addresses **Name resolution**
- Registration of alias (another name) of host name and mail server name
- DNS round-robin (load distribution)
    - Matching set of the public host name and multiple mirror servers' IP addresses
    - Shifts the order of list after each request (The client uses the first address)
##### Structure of DNS
'DNS is a distributed database'
**Why isn't it a centralized system?**
- Lack of durability against failure (single point of failure)
- Access concentration to a server
- Much delay due to the need for remote access
- Complexity of maintenance and updates
**=> In summary, it is not scalable!**

Distributed model (classification of name servers)
- Root DNS server (There are only 13 "types")
    - It knows IP addresses of top-level DNS servers
    - In the past, 'there were only 13 of them', however they are multiplexed at present
- Top-level domain DNS servers
    - They manage top-level domains such as jp, com, edu
    - In general, they are managed and operated by an organization that is called 'registrar'
- Auth-DNS servers (Authoritative DNS servers)
    - They can translate host names (domain names) and IP addresses
    - Each host is registered in at least one auth-DNS server.
- (Local DNS server)
    - It is used by each organization such as schools, companies, etc.
    - Each host, at first, accesses the local DNS server that caches data from the corresponding auth-DNS server