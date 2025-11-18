Continuing from the last (5th) lecture...

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
    - Each host, at first, accesses the local DNS server that may cache data from the corresponding auth-DNS server
-  DNS iterative query and DNS recursive query
    - DNS iterative query: Clarilfy the name server which is in charge
    - DNS recursive query: Send name resoliving requests to the name server
Normally, DNS recursive query is used between the end thost to the local DNS server. Local DNS servers use DNS recursive query to handle name resolving requests.
- The local DNS server chaches the information that are send to hosts: **DNS cache**
    - Chaches are deleted after designated time frame.
- DNS record
    - The distributed database which contain resource record (RR).
        - **zone file**: The set of RR that auth-servers control.
    - It is consist of 4 filelds (name, value, type, ttl)
        - Type A record: name and value correspond to the host name and IP address (AAAA records for IPv6) respectively
        - Type NS record: name and value correspond to the domain name and the its auth-server's host name respectively
        - Type CNAME record: Another name of a host name
        - Type MX record: Information anout the mail server of the name

#### Attack against DNS server
**Cache poisoning**
1. An attcker request name resolving to the DNS cache server
2. THe DNS cahe server does recursive query
3. An attacker instantaneously, send fake response (UDP segment)
4. The DNS cache serber memorizes the fake domain and discards the true response from upper servers

**Requirements for the attack establishment**
- IP address is mimicked the requesting DNS server
- Use same port as query
- Use same ID (16 bits) as query
- Respond faster than the real DNS server

In fact, there are few chances for these attack because chances appears only when the cache expire. (From this view point, some consider more effective attack.)

### Contents distribution
If contents are in only one server,
- Transmission delay increases
- Server loads also increase

**Contents distribution**
- Copy contents on multiple servers on the internet
- Find the server that can serve the contents the fastest to the user.

#### Web cache
web cache and proxy server.
- Respond to the HTTP request from clients instead of the original server
- Contains copies of object that are accessed recently, on its own disc.
- If the web cache has the copy, users downlods it from the web cache.
    - Users send HTTP request to the web cache
    - If the web cache does not have the copy, it sends HTTP request to the original server.

#### CDN
CDN (Content Distribution Network)
- The buisuiness model that occured late 90's.
(ex) Akamai: places 350k servers
- Each CDN service company place mutiple CDN server on the internet.
    - place CDN server intra ISP (internet service provider) or connect it to ISP
- Netflix and Youtubeuse their own private CDN for their services.
- Each CDN service company copies customers' contents on their own CDN servers (per each update).
    - Distinguish contents between that are copied on their CDN server and that are ignored.
    - The latter ones are transmitted to a **CDN distributing server**.
    - The CDN distributing server distributes the content to every single CDN servers (uses exclusive channel).
    - The appropriate CDN server respond to the users' HTTP requests.
        - Based on routing table and eastimated RTT (Round-trip time), each ISP register the IP address of the CDN server that can realize the shortest respond time.

(ex) contents provider: www.foo.com
CDN service company: www.cdn.com
- foo wants to serve only JPEG files using CDN
- foo replaces references all JPEG file in HTML objects, or designates CNAME in the DNS record.
1. Users send HTTP request to www.foo.com and receive a HTML file.
2. Read HTML file finding links such as http://www.cdn.com/www.foo.com/sports/a.jpg. Or, send query to foo.com again being redirected to cdn.com based on CNAME of DNS.
3. Ask IP address of www.cdn,com to the DNS.
4. The auth-sever of cdn.com select the appropriate CDN server for the user, and return its IP address.
5. The browser send HTTP request to the received IP address.
This IP address is memorized by DNS cache.

    

