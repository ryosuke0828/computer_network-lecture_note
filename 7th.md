## Transport Layer
### Transport layer services
- Communicate with the application process to directly deliver a service
    - It is incorporated into end systems *(OS)* that have applications
    - It is not normally incorporated into routers in a network.
- Deliver **logical communication** between application process
    - The application does not concern the structure of physical communication infrastructure

#### Relationships between transport layers and network layers
- Network layers: Deliver logical communication between **hosts**
    - It is incorporated within routers that relay messages
- Transport layers make good use of packet delivery services by network layers

#### Transport layers of an internet
- The minimum functions (Both TCP and UDP realize)
    - Transmitting services between processes (multiplexing and demultiplexing)
    - Transmitting error detection of a segment
- UDP (User Datagram Protocol)
    - Connection-less service that does not ensure reliability
    - It only realizes the minimum functions
- TCP (Transmission Control Protocol)
    - The connection-oriented service that is highly reliable
    - It contains not only the minimum functions but also functions such as error correction, segment re-ordering and congestion control.

### Multiplexing and Demultiplexing
- Multiplexing: Lump data that are sent to transport layer from various **sockets** together before handing them to network layer
    Socket: Interface between application layer and transport layer
- Demultipexing: Divide data that are handed from network layer and send them to appropriate sockets

- Port: Number to distinguish sockets ($0 \sim 65535 = 2^{16} - 1$)
    - Port $0 \sim 1023$ are reserved as well-known port numbers
    - The same number can be used differently between TCP and UDP
In general, a client randomly selects a port number (1024 and later) when creating a UDP/TCP socket. Conversely, a server normally uses a well-known port number.

#### Multiplexing and demultiplexing of UDP
- Client: Add start-point port number and end-point port number to header (bind to a socket) then multiplex it
- Server: Select a socket based on end-point port number (and end-point IP address)
    - Segments from different clients are handed to the same socket. The start-point port number is also used as the end-point port number of the reply.

#### Multiplexing and demultiplexing of TCP
- Client: Same as that of UDP
- Server: Select a socket based on start-point IP address, start-point port number, end-point IP address and end-point port number (all of them)

### UDP
- 8 byte header + payload (application layer message)

<table>
<tr>
<th colspan="2" align="center">← 32 bit →</th>
</tr>
<tr>
<td>start-point port number</td>
<td>end-point port number</td>
</tr>
<tr>
<td>segment length</td>
<td>check sum</td>
</tr>
<tr>
<td colspan="2" align="center">application layer message</td>
</tr>
</table>


- Error detection is done at the receiver side using checksum (1's complement of the result of adding all 16-bit words).
- Do nothing else.

Merits
1. Data and transmission timing can be controlled at application level
2. Do not have to establish the connection and thus fast
3. Do not require connection control and thus less load on servers
4. Smaller header size (8 bytes for UDP and 20 bytes for TCP)

### Reliable data transfer
- No error (bit reversal) and ordered data according to transmission order
- **ARQ (Automatic Repeat reQuest)**
    - The control mechanism for realizing reliable data transfer
    - Do delivery confirmation and error detection to resend if needed

In the following, we consider requirements by making the situation complicated step by step
**Level 1: Packet breaking down (bit error) communication channel.** In this case, we need error detection and delivery confirmation.
- ACK (Acknowledgement): notify if delivered correctly.
- NACK (Negative ACK): notify if NOT delivered properly.
- Stop and wait
    - Send packets one by one
        - Receiver's buffer size is only 1
    - Send next packet only after receiving ACK/NACK

**Level 1.1: ACK/NACK can also be errored.** In this case, we cannot know if delivery succeeded. => Attach **sequence number** to a packet to distinguish resent packets and new packets
