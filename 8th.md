### Reliable data transfer
- No error (bit reversal) and ordered data according to transmission order
- **ARQ (Automatic Repeat reQuest)**
    - The control mechanism for realizing reliable data transfer
    - Do delivery confirmation and error detection to resend if needed

In the following, we consider requirements by making the situation complicated step by step
**Level 1: Packet breaking down (bit error) communication channel.** In this case, we need error detection and delivery confirmation.
- ACK (Acknowledgement): notify if delivered correctly.
- NACK (Negative ACK): notify if NOT delivered properly.
- **Stop and wait**
    - Send packets one by one
        - Receiver's buffer size is only 1
    - Send next packet only after receiving ACK/NACK

**Level 1.1: ACK/NACK can also be errored.** In this case, we cannot know if delivery succeeded. => Attach **sequence number** to a packet to distinguish resent packets and new packets. In the case of stop-and-wait, sequence number does not have to more than 1 bit.

**Level 1.2: Don't use NACK** If the sender get ACK for same sequence number twice, its effect is same as NACK. => **duplicate NACK**

**Level 2: Packet loss communication channel** 
- In this case, packet may not be arrive at the opponent side. => set **time out term**.
- If the sender may not recieve ACK in the designated time out term, it is considerd as the failure and resend it.
- Time out term has to be longer than delay term.

#### Stop-and-wait (SW)
- The sender side
    - Attach sequnce number (1 bit) to each packet.
    - Set time out term
1.  Send packets by order
2. Wait till recieve ACK
    2.1 Recieve ACK in time out term => go to 1
    2.2 If not, go to 1 (resend)

- The reciever side
1. Once recieve a packet, check if there are errors.
2. If not, send ACK packet with the sequence number.
    
- The base performance of SW (without any error, ignore transfer delay of ACK)
    - Packet length: $L$ (bit), channel speed $R$ (bps), round trip delat $RTT (s)

**Time for tranfering one packet**
$$RTT + \frac{L}{R}$$

**Through put**
$$\theta = \frac{L}{RTT + \frac{L}{R}}$$

**channel occupied rate**
$$U = \frac{\theta}{R} = \frac{L}{R \times RTT + L}$$
※ $R \times RTT$: Bandwidth delay product (BDP)

If BDP increases, channel usability decreases.

### Go-Back-N (GBN)
- Can send successive maximum $N$ packets without waiting for ACK recieval. => sliding window protocol
- If communication failure (error, loss, order change) occurs, return to the packet and do it again.
※ Reciever's buffer size is 1, failed packets are discareded.

- The sender side
    - Attach sequence number to a each packet ($k$ bits)
    - Configure the window size ($N<2^k$) and time out term.
    - $F$ number of packets that are sent but ACK is not recieved.
1. Send packets following order until $N=F$.
2. Wait for ACK of each packet
    2.1 Recieve ACK in time out term, go to 1.
    2.2 If not, go to 1 to resend packets after the packet (includes that packet as well)

- The reciever side
1. Once catch a packet, check if 
    - it has error
    - correct order
2. No error, correct order => send back ACK with respective sequence number. If not, discard the packet send back ACK with sequence number of the last correct packet.

**Cumulate ACK** The concept that packets before the largest correct sequnce number are all correctly recieved.
