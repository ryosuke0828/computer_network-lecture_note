### Selective repeat (SR)
- How SR protocol works
    - Send up to $N$ successive packets without waiting for ACK.
    - Receiver's buffer size is $N$.
    - If sending failures are detected, resend only the packet.
- Sender side
    - Attach sequence number ($k$ bit) to a packet.
    - Register window size $N (N \leq 2^{k-1})$ and time out period.
    - $n$: The smallest sequence number of a packet that is sent but ACK not received.
  
1. Send successive $n+N-1$ packets.
2. Wait for ACK until time out period.
2.1. If the sender receives ACK in time, go to 1.
2.2. If it is time out period, resend the packet and go to 2.

- Receiver side
1. If catching a packet, check if it contains error.
2. If there is no error, send back ACK with its sequence number.

## TCP
TCP (Transmission Control Protocol)
- Connection-oriented reliable transport protocol on Internet.
- There are various minor-changed models
- Realization of basic action rules depends on how it is implemented.

### TCP connection and TCP segment
- Full duplex
- point to point
- Segment unit transport
    - Segment = TCP header + AP layer data
    - Maximum segment size (MSS)
        - The maximum size of AP layer data in a segment
        - e.g., 1460 bytes for Ethernet LAN.
- Control information included in header
  - start/end point port number
  - sequence number, ACK number
  - SYN, FIN, ACK flags

  - sequence number: byte unit successive number
      - corresponds to the first byte of transmitted data
      - starts with random number (to detect scam)
  - ACK number: sequence number that corresponds to the **next byte that should be received next**. => **Cumulative ACK** : ALL bytes before the ACK number are received successfully

**Connection establishment**: 3-way hand shake
1. The client sends SYN segment with initial value of sequence number.
2. The server sends back SYN segment with the initial value of the sequence number and ACK for received segment.
3. The client sends ACK segment with ACK number for the received SYN/ACK segment.

**Connection end**
1. A side that wants to end the connection (A) sends FIN segment to the other (B).
2. Once B receives FIN segment, send back ACK segment to A.
3. If B has any further data, connection still continues.
4. If there isn't any further data, B sends FIN segment to A.
5. A sends ACK for received FIN segment and becomes waiting status.
6. B cuts the connection after receiving the ACK segment. (If time out, resend FIN segment)

<u>Delayed ACK</u>
By delaying ACK, make good use of bandwidth.
<u>Implementation example of TCP ACK</u> (There are no explicit rules in RFC)
1. Send segments in correct order, and there are no segments in "waiting for ACK sending" status, Postpone ACK sending for up to 500 ms, within this period, if the following 2,3 do **not** occur, send ACK.
2. Send segments in correct order and there are segments in "waiting for ACK sending", send cumulative ACK for two segments right away.
3. Receive segments with unexpectedly larger sequence number, send duplicate ACK for expected sequence number.

4. When receiving segments that fill loss part of received data, if it contains the smallest number of loss part, send ACK.

- Selective ACK option (SACK)
    - Configure when to send a SYN segment
    - When to send duplicate ACK, also send information about correctly received segments. (First and last byte number of correctly received block)