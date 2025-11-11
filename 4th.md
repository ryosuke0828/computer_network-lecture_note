### Web and HTTP
##### Non-persistent connection and Persistent connection
- Non-persistent connection
    - It transfers one web object within one TCP connection.
    - It is used in HTTP/1.0
    - Normally, a browser can contain 5 - 10 TCP connection at parallel. However, in case they are fully used, each object are transfered one by one.
- Weak points of the non-persistent connection:
    - The server has to controll a plenty of TCP connections and thus it use much resources
    - Each object requires 2 RTT (1 RTT for connection establishment). RTT (round trip time): Delaying time between a client and a server.
- Persistent connection
    - It transfers mutiple web objects within one TCP connection.
    - It is used default in HTTP/1.1
    - A server keeps TCP connection after a response
    - If the connection is not used in designated time, server let the connection go.
- Pipeline processing
    - Non-pipeline processing:
    A client receives a response, then sends another request. (The server becomes idle for a while after it responds.)
    - Pipeline processing:
    A client sends request before it receives a response to former requests. The server process mutiple objects. (Used in HTTP/1.1)
  
#### HTTP message
- There are two types of HTTP messages: Request and response
Elements of a request message
    - Request row: HTTP method (GEt, POST, HEAD and etc...), request target (URL), HTTP version.
    - Header row: it stores user agents (iOS or windows, safari or chrome type shi) in key:value form
    - Body

Elements of a response message
    - Status row: status code (200, 400, 500 and etc...) + status strings
    - Header row: It stores Via (such as proxy server) and Content-length (data size) in key:value form
    - Body

#### HTTP/2
The new version of HTTP which is RFC documented in May, 2015.
- **Stream**: The virtual alternate direction sequence of multiple requests and multiple responses with a connection
    - Multiplying stream enables to sending/receiving objects at the same time. (Solution of HoL blocking)
    ※ HoL (head of line) blocking issue: Large objects cause wait because responses have to be returned according to request order.

- Compact HTTP header by using HPACK
    - Send only difference after second communication
- HTTP/2 frame: Communication by binary data in the form of *frame*.
    - Divide HTTP header and data into frames (it also can use header compacting).
    - Frame dividing and compacting is transparent to HTTP/1.x messages.
    (These are located below HTTP layer. The application developer has not care about those changes.)
- Server push (Sends data for response to client in advance.)
- Priority control of stream.
- It can be used in any other than TCP.

#### HTTP streaming and DASH
**DASH** (Dynamic Adaptive Streaming over HTTP)
- A video is a sequence of images (24 or 30 frames/s)
- Encodes video file by several different bit rates
    - low-quality: 100kbps, streaming: 3Mbps, 4K: 10Mbps
- Divide videos into **segments** and contains in server. (1 segment consits of seconds to tens seconds data)
- Clients determine which segments to require *dynamically*.
- A HTTP server contains MPD (Media Presentation Description).
    - URLs of each version of segments are written.
    - Client first request and fetch MDP from server.