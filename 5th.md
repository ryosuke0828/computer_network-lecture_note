#### FTP (File Transfer Protocol)
It is a protocol for out-of-vount method.
- controlling connection: port 21, persistent connection type
    - user authentification, password, commands uch as cd, put and get
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
Use SMTP in communications between a start user agent and a server, or among servers.
Use mail access protocols between a mail server and a end user agent.

##### SMTP
Transfer message from the start mail server to the end mail server
- Use 7 bits ASCII code as a data format
1. Client SMTP connect port 25 of server SMTP using TCP
2. Hand-shle in the application layer
    (a) The client let the server know the sender address, and the server response with sending grant.
    (b) The client let the server know the destination address, and the server notify if the destination address actually exists
3. The client notify the start of data transporting, and actually start it soon after it receives the server's response.
4. Once transmitting finishes, cut the connection
- TCP connection is persistent connection type.
    If there are miltiple mails to a one server, They are transported using only one connection.