## Web and HTTP
#### Non-persistent connection and Persistent connection
- Non-persistent connection
    - It transfers one web object within one TCP connection.
    - It is used in HTTP/1.0
    - Normally, a browser can maintain 5 - 10 TCP connections in parallel. However, in case they are fully used, each object is transferred one by one.
- Weak points of non-persistent connection:
    - The server has to control many TCP connections and thus it uses many resources
    - Each object requires 2 RTT (1 RTT for connection establishment). RTT (round trip time): Delay time between a client and a server.
- Persistent connection
    - It transfers multiple web objects within one TCP connection.
    - It is used by default in HTTP/1.1
    - A server keeps the TCP connection open after a response
    - If the connection is not used within a designated time, the server closes the connection.
- Pipeline processing
    - Non-pipeline processing:
    A client receives a response, then sends another request. (The server becomes idle for a while after it responds.)
    - Pipeline processing:
    A client sends a request before it receives a response to previous requests. The server processes multiple objects. (Used in HTTP/1.1)
  
### HTTP message
- There are two types of HTTP messages: Request and response
Elements of a request message
    - Request line: HTTP method (GET, POST, HEAD, etc.), request target (URL), HTTP version.
    - Header lines: Stores user agents (iOS or Windows, Safari or Chrome, etc.) in key:value form
    - Body

Elements of a response message
    - Status line: status code (200, 400, 500, etc.) + status strings
    - Header lines: Stores Via (such as proxy server) and Content-length (data size) in key:value form
    - Body

### HTTP/2
The new version of HTTP which was documented in RFC in May 2015.
- **Stream**: The virtual bidirectional sequence of multiple requests and multiple responses within a connection
    - Multiplexing streams enables sending/receiving objects at the same time. (Solution to HoL blocking)
    ※ HoL (head of line) blocking issue: Large objects cause waiting because responses have to be returned according to request order.

- Compact HTTP header using HPACK
    - Send only differences after the second communication
- HTTP/2 frame: Communication using binary data in the form of *frames*.
    - Divide HTTP header and data into frames (it can also use header compression).
    - Frame division and compression is transparent to HTTP/1.x messages.
    (These are located below the HTTP layer. Application developers do not need to care about these changes.)
- Server push (Sends data for response to client in advance.)
- Priority control of streams.
- It can be used with protocols other than TCP.

### HTTP streaming and DASH
**DASH** (Dynamic Adaptive Streaming over HTTP)
- A video is a sequence of images (24 or 30 frames/s)
- Encodes video file at several different bit rates
    - low-quality: 100kbps, streaming: 3Mbps, 4K: 10Mbps
- Divide videos into **segments** and store them on the server. (1 segment consists of several to tens of seconds of data)
- Clients determine which segments to request *dynamically*.
- An HTTP server contains the MPD (Media Presentation Description).
    - URLs of each version of segments are written.
    - Client first requests and fetches the MPD from the server.