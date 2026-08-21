
# Networking for Quant Developer Interviews

For a **quant developer / low-latency trading interview**, networking questions are usually less about memorizing the OSI model and more about understanding:

- How bytes move between machines
- TCP vs UDP
- Sockets
- Packets and frames
- Multicast
- Latency
- Kernel networking
- NICs
- Buffering
- Packet loss
- Market data distribution

---

# 1. High-Level Mental Model

Suppose a trading application wants to send an order:

```text
Application
    ↓
Socket API
    ↓
Kernel networking stack
    ↓
NIC
    ↓
Ethernet
    ↓
Switches / Routers
    ↓
Destination NIC
    ↓
Destination kernel
    ↓
Destination application
```

Your application might simply call:

```cpp
send(sock, buffer, len, 0);
```

But underneath, many layers are involved.

---

# 2. Networking Layers

You do not need to obsess over all 7 OSI layers, but you should understand the important ones.

```text
Application       FIX / market data protocol / HTTP
Transport         TCP / UDP
Network           IP
Data Link         Ethernet
Physical          electrical / optical signals
```

A useful mental model is:

```text
Application message
    ↓
TCP/UDP segment
    ↓
IP packet
    ↓
Ethernet frame
    ↓
bits on the wire
```

Each layer adds its own header.

For example:

```text
+-------------------+
| Ethernet Header   |
+-------------------+
| IP Header         |
+-------------------+
| TCP/UDP Header    |
+-------------------+
| Application Data  |
+-------------------+
```

This is called **encapsulation**.

---

# 3. MAC Addresses vs IP Addresses vs Ports

These are three separate concepts.

## MAC Address

A MAC address identifies a network interface at the Ethernet layer.

Example:

```text
00:1A:2B:3C:4D:5E
```

Used mainly for communication on a local network.

---

## IP Address

An IP address identifies a host/interface across networks.

Example:

```text
192.168.1.50
```

IPv4 is 32 bits.

---

## Port

A port identifies a particular application or service on a machine.

Example:

```text
192.168.1.50:8080
```

Think:

```text
IP address → which machine?
Port       → which application on that machine?
```

---

# 4. What Is a Socket?

A **socket** is the operating system abstraction used by processes to communicate over a network.

Example:

```cpp
int fd = socket(AF_INET, SOCK_STREAM, 0);
```

Here:

```text
AF_INET       → IPv4
SOCK_STREAM   → TCP
```

For UDP:

```cpp
int fd = socket(AF_INET, SOCK_DGRAM, 0);
```

A socket is represented by a file descriptor on Unix-like systems.

You can interact with it using functions like:

```cpp
socket()
bind()
listen()
accept()
connect()
send()
recv()
close()
```

---

# 5. TCP

TCP stands for **Transmission Control Protocol**.

TCP provides:

- Reliable delivery
- Ordered delivery
- Error detection
- Retransmission
- Flow control
- Congestion control
- Connection-oriented communication

The most important interview idea:

> TCP is a reliable, ordered **byte stream**.

It is not message-oriented.

---

# 6. TCP Is a Byte Stream

Suppose you do:

```cpp
send(fd, "ABC", 3, 0);
send(fd, "DEF", 3, 0);
```

The receiver is not guaranteed to receive two matching chunks.

It could get:

```text
recv() → "ABCDEF"
```

or:

```text
recv() → "AB"
recv() → "CDEF"
```

or:

```text
recv() → "A"
recv() → "BCD"
recv() → "EF"
```

TCP only guarantees that the receiver eventually sees:

```text
ABCDEF
```

in order.

Therefore, application protocols usually need **message framing**.

For example:

```text
[length][payload]
```

Example:

```text
0005HELLO
0003CAT
```

The receiver first reads the length, then reads exactly that many bytes.

---

# 7. TCP Connection Establishment

TCP uses a three-way handshake:

```text
Client                         Server

   -------- SYN ------------->

   <------ SYN-ACK ------------

   -------- ACK ------------->
```

After this, the connection is established.

The packets contain TCP sequence numbers so both sides can track transmitted bytes.

---

# 8. TCP Sequence Numbers

TCP numbers bytes in the stream.

Suppose:

```text
Sequence number = 1000
payload = 500 bytes
```

Then the next segment might start at:

```text
1500
```

The receiver acknowledges what it has received.

Example:

```text
Sender:
SEQ=1000, 500 bytes

Receiver:
ACK=1500
```

Meaning:

> I have received everything before byte 1500.

---

# 9. TCP Retransmission

Suppose:

```text
Sender:

Packet 1000
Packet 1500
Packet 2000
```

and packet 1500 is lost.

The receiver detects that data is missing.

TCP will eventually retransmit the missing data.

This is why TCP is reliable.

But retransmission introduces latency.

For low-latency trading, that can be undesirable.

---

# 10. Head-of-Line Blocking

Suppose TCP data arrives like:

```text
Packet 1
Packet 2  ← lost
Packet 3
Packet 4
```

Even though packets 3 and 4 arrived, TCP must present the byte stream to the application in order.

So the application effectively waits for packet 2.

```text
Network received:

1  [missing]  3  4

Application sees:

1
(wait)
2
3
4
```

This is called **head-of-line blocking**.

This is one reason real-time market data often uses UDP instead.

---

# 11. UDP

UDP stands for **User Datagram Protocol**.

UDP provides much weaker guarantees.

It is:

- Connectionless
- Message-oriented
- Unreliable
- Potentially unordered
- Lightweight

UDP does not automatically provide:

- Retransmission
- Ordering
- Flow control
- Congestion control
- Connection management

---

# 12. UDP Is Message-Oriented

Suppose you send:

```text
Datagram A
Datagram B
Datagram C
```

The receiver deals with separate datagrams.

Unlike TCP, message boundaries are preserved.

But the receiver might see:

```text
A
C
```

because B was dropped.

Or theoretically:

```text
B
A
C
```

because packets may be reordered.

---

# 13. TCP vs UDP

| TCP | UDP |
|---|---|
| Connection-oriented | Connectionless |
| Reliable | Unreliable |
| Ordered | No ordering guarantee |
| Byte stream | Datagrams/messages |
| Retransmission | No automatic retransmission |
| Flow control | No built-in flow control |
| Congestion control | No built-in congestion control |
| More protocol overhead | Less protocol overhead |

A good interview explanation:

> TCP is useful when correctness and reliable delivery matter more than the cost of retransmissions and ordering. UDP is useful when latency is more important and the application can tolerate or handle packet loss itself.

---

# 14. Why Market Data Often Uses UDP

Imagine an exchange needs to distribute the same market data to hundreds of trading firms.

With TCP:

```text
Exchange
   ├── TCP connection → Firm A
   ├── TCP connection → Firm B
   ├── TCP connection → Firm C
   ├── TCP connection → Firm D
   └── ...
```

The exchange maintains separate connection state and sends data separately.

With UDP multicast:

```text
                    ┌── Firm A
                    ├── Firm B
Exchange → Network ─├── Firm C
                    ├── Firm D
                    └── Firm E
```

The exchange can send one stream which the network distributes to many receivers.

This is very useful for:

```text
NYSE/Nasdaq market data
options feeds
futures feeds
crypto market data
```

---

# 15. UDP Multicast

Normal UDP is usually **unicast**:

```text
Sender → Receiver
```

Multicast is:

```text
                 ┌→ Receiver A
Sender → Network ├→ Receiver B
                 └→ Receiver C
```

Receivers join a multicast group.

Conceptually:

```cpp
setsockopt(
    socket_fd,
    IPPROTO_IP,
    IP_ADD_MEMBERSHIP,
    ...
);
```

A multicast address may look like:

```text
239.1.2.3
```

A trading application subscribes to that group and receives the market data packets.

---

# 16. Sequence Numbers in Market Data

UDP itself does not guarantee reliable delivery.

Therefore exchanges often include sequence numbers in their application protocol.

Suppose you receive:

```text
100
101
102
104
105
```

You immediately know:

```text
103 is missing
```

Your market data handler can detect:

```cpp
if (sequence != expected_sequence) {
    // packet loss
}
```

---

# 17. Gap Recovery

Suppose your feed receives:

```text
5000
5001
5002
5005
```

You know you lost:

```text
5003
5004
```

A trading system may then use a separate **recovery channel**.

Conceptually:

```text
Fast UDP multicast feed
        +
Slower reliable TCP recovery connection
```

Normal path:

```text
UDP:
5000
5001
5002
5003
5004
5005
```

Packet-loss path:

```text
UDP:
5000
5001
5002
5005

detect gap

TCP recovery request:
"Give me 5003-5004"
```

This combines:

- UDP's speed
- TCP's reliability when recovery is required

This is a common design pattern in financial market data systems.

---

# 18. Snapshot + Incremental Updates

Market data is frequently represented as:

```text
Snapshot
+
incremental updates
```

For example, order book snapshot:

```text
AAPL

Bid             Ask

$199.99 100     $200.00 200
$199.98 300     $200.01 150
$199.97 500     $200.02 250
```

Then updates might say:

```text
Add bid $199.99 x 50
Delete ask $200.00 x 100
Add ask $200.03 x 200
```

Rather than transmitting the entire book every time, the exchange sends changes.

This saves huge amounts of bandwidth.

---

# 19. Why Missing One Packet Can Be Dangerous

Suppose your book is:

```text
Bid:
$100 × 100

Ask:
$101 × 100
```

Then updates arrive:

```text
1: remove $101 ask
2: add $102 ask
```

If you miss update 1 but receive update 2, your local book could incorrectly become:

```text
Ask:
$101 × 100
$102 × 100
```

even though the real market is:

```text
Ask:
$102 × 100
```

Your local state is now corrupted.

That is why sequence numbers and recovery are extremely important.

---

# 20. Ethernet

Ethernet operates at Layer 2.

An Ethernet frame roughly looks like:

```text
+----------------------+
| Destination MAC      |
+----------------------+
| Source MAC           |
+----------------------+
| EtherType            |
+----------------------+
| Payload              |
| usually IP packet    |
+----------------------+
| CRC                  |
+----------------------+
```

Ethernet gets data between devices on a local network.

---

# 21. Ethernet Frame vs IP Packet

An IP packet is carried inside an Ethernet frame.

```text
Ethernet Frame
┌─────────────────────────────┐
│ Ethernet header             │
│                             │
│   IP Packet                 │
│   ┌───────────────────────┐ │
│   │ IP Header             │ │
│   │                       │ │
│   │ UDP/TCP Segment       │ │
│   │ ┌───────────────────┐ │ │
│   │ │ TCP/UDP header    │ │ │
│   │ │ Application data  │ │ │
│   │ └───────────────────┘ │ │
│   └───────────────────────┘ │
└─────────────────────────────┘
```

Think:

```text
Ethernet → local hop
IP       → end-to-end routing
TCP/UDP  → process-to-process transport
```

---

# 22. Switch vs Router

## Switch

A switch operates primarily using MAC addresses.

```text
Computer A
    |
  Switch
 /     \
B       C
```

The switch maintains something like:

```text
MAC Address        Port

AA:AA:AA:AA        1
BB:BB:BB:BB        2
CC:CC:CC:CC        3
```

So if a frame is for:

```text
BB:BB:BB:BB
```

the switch forwards it through port 2.

---

## Router

A router operates primarily using IP addresses.

Routers move packets between different networks.

Example:

```text
192.168.1.x
     |
   Router
     |
10.20.30.x
```

The router maintains a routing table describing where packets should go.

---

# 23. ARP

Suppose you know:

```text
Destination IP:
192.168.1.50
```

But Ethernet needs a MAC address.

How do you determine it?

Using **ARP**, the Address Resolution Protocol.

Your machine broadcasts something like:

```text
Who has 192.168.1.50?
```

The target responds:

```text
192.168.1.50 is at:

AA:BB:CC:DD:EE:FF
```

Your OS caches this mapping in an ARP table.

Conceptually:

```text
IP                 MAC

192.168.1.50       AA:BB:CC:DD:EE:FF
```

---

# 24. NIC

NIC stands for **Network Interface Card**.

The NIC is the hardware that interfaces your computer with the network.

```text
CPU
 |
RAM
 |
PCIe
 |
NIC
 |
Ethernet
```

It receives Ethernet frames from the wire and transfers them into system memory.

Similarly, outgoing data moves:

```text
Application
    ↓
kernel
    ↓
memory
    ↓
NIC
    ↓
network
```

---

# 25. DMA

NICs often use **DMA**, Direct Memory Access.

Without DMA:

```text
NIC
 ↓
CPU manually copies every byte
 ↓
RAM
```

That would consume CPU resources.

With DMA:

```text
NIC ─────────→ RAM
      DMA
```

The NIC can transfer data directly into memory.

Then it notifies the CPU.

This is fundamental for high-speed networking.

---

# 26. Interrupts

When the NIC receives a packet, it can notify the CPU using an interrupt.

Conceptually:

```text
Packet arrives
    ↓
NIC receives packet
    ↓
NIC DMA writes to memory
    ↓
NIC interrupts CPU
    ↓
kernel handles packet
```

But there is a problem.

Suppose you receive:

```text
10 million packets/sec
```

If every packet causes an interrupt:

```text
10 million interrupts/sec
```

that would be extremely expensive.

---

# 27. Interrupt Coalescing

NICs may batch packet notifications.

Instead of:

```text
packet → interrupt
packet → interrupt
packet → interrupt
packet → interrupt
```

you can do:

```text
packet
packet
packet
packet
    ↓
one interrupt
```

This improves throughput.

But it can increase latency because the NIC waits before interrupting the CPU.

This creates an important low-latency tradeoff:

```text
more batching
    ↓
better throughput
but
higher latency
```

---

# 28. Kernel Networking Receive Path

A simplified packet receive path:

```text
Network
   ↓
NIC
   ↓
DMA into RX ring buffer
   ↓
kernel network driver
   ↓
Ethernet processing
   ↓
IP processing
   ↓
TCP/UDP processing
   ↓
socket receive buffer
   ↓
recv()
   ↓
application
```

Every step introduces some latency.

---

# 29. Receive Ring Buffers

NICs commonly use ring buffers.

Conceptually:

```text
RX Descriptor Ring

[buffer]
[buffer]
[buffer]
[buffer]
[buffer]
   ↑
  NIC
```

Each descriptor points to memory where the NIC can place an incoming packet.

When packets arrive:

```text
NIC writes packet → buffer
NIC advances descriptor
```

The kernel consumes these buffers.

---

# 30. Why Ring Buffers?

A ring buffer allows reuse of a fixed amount of memory.

```text
0 → 1 → 2 → 3 → 4
↑               ↓
└───────────────┘
```

Advantages:

- No allocation for every packet
- Predictable memory usage
- Good cache behavior
- Fast producer/consumer model

Networking systems use them extensively.

---

# 31. Kernel Bypass

Normal networking:

```text
NIC
 ↓
kernel
 ↓
socket buffer
 ↓
system call
 ↓
application
```

This has overhead:

- interrupts
- context switches
- kernel protocol processing
- data copies
- scheduling
- system calls

Low-latency systems may use **kernel bypass**.

Conceptually:

```text
NIC
 ↓
user-space memory
 ↓
trading application
```

Technologies include:

```text
DPDK
Solarflare/OpenOnload
RDMA-related technologies
vendor-specific low-latency APIs
```

---

# 32. Why Kernel Bypass Helps

Without bypass:

```text
NIC
 ↓
kernel buffer
 ↓
socket buffer
 ↓
application
```

Potentially:

```text
copy
copy
system call
context switch
```

With bypass:

```text
NIC
 ↓
application-visible memory
```

You remove or reduce some layers.

The goal is:

```text
lower latency
lower jitter
more predictable performance
```

---

# 33. Polling vs Interrupts

Normal system:

```text
packet arrives
    ↓
interrupt CPU
```

Low-latency systems may instead **poll**:

```cpp
while (true) {
    if (packet_available()) {
        process_packet();
    }
}
```

This burns CPU continuously.

But when a packet arrives, you can process it immediately.

Tradeoff:

```text
Interrupts:
+ CPU efficient
- wakeup latency

Polling:
+ lower/predictable latency
- burns CPU
```

Trading systems often prefer polling because latency matters more than saving CPU cycles.

---

# 34. Busy Waiting

Polling is essentially busy waiting:

```cpp
while (!ready) {
}
```

Normally this looks wasteful.

In a latency-critical system, though, dedicating one CPU core to:

```text
market data feed
```

may be perfectly reasonable.

You spend CPU to eliminate:

```text
scheduler delays
interrupt delays
thread wakeups
```

---

# 35. CPU Affinity

Another important low-latency idea is **CPU affinity**.

You can pin a thread to a particular CPU core.

Instead of:

```text
thread:
core 1 → core 4 → core 7 → core 2
```

you force:

```text
thread → core 3
```

Why?

Because moving between CPUs can hurt:

- cache locality
- scheduling predictability
- latency

A market data thread may therefore be pinned to one core.

---

# 36. RSS

RSS stands for **Receive Side Scaling**.

A multi-core machine might receive many network flows.

Instead of sending all packets to one CPU:

```text
NIC → CPU 0
```

the NIC hashes flows across multiple receive queues:

```text
              ┌→ RX queue 0 → CPU 0
NIC packets ──├→ RX queue 1 → CPU 1
              ├→ RX queue 2 → CPU 2
              └→ RX queue 3 → CPU 3
```

The hash can use fields such as:

```text
source IP
destination IP
source port
destination port
```

This improves parallelism.

---

# 37. Cache Locality and Networking

Suppose packets for the same feed constantly move between cores:

```text
Packet 1 → Core 1
Packet 2 → Core 4
Packet 3 → Core 7
```

Data structures like your order book may bounce between CPU caches.

That causes cache-coherence traffic.

Better:

```text
same feed
    ↓
same RX queue
    ↓
same CPU core
    ↓
same thread
```

Then hot data stays in the CPU cache.

This is very important in low-latency systems.

---

# 38. NUMA

Large servers may have multiple CPU sockets.

Each CPU has memory closer to it.

Example:

```text
CPU socket 0 ─── local RAM 0
      |
      └──── slower access ─── RAM 1

CPU socket 1 ─── local RAM 1
```

This is **NUMA**:

```text
Non-Uniform Memory Access
```

For low latency, you ideally want:

```text
NIC
 ↓
CPU core
 ↓
memory
```

all close together on the same NUMA node.

Otherwise you incur additional inter-socket traffic.

---

# 39. TCP Flow Control

TCP has **flow control** so a fast sender does not overwhelm a slow receiver.

The receiver advertises a receive window.

Example:

```text
Receiver:
"I have 64 KB of buffer space available."
```

The sender limits how much unacknowledged data it sends accordingly.

Conceptually:

```text
Sender speed
     ↓
limited by receiver window
```

This is different from congestion control.

---

# 40. TCP Congestion Control

Flow control protects the receiver.

Congestion control protects the network.

If the network is congested:

```text
Sender
 ↓↓↓↓↓
Network bottleneck
 ↓
packet drops
```

TCP reduces its sending rate.

Mechanisms include concepts such as:

```text
congestion window
slow start
loss detection
rate reduction
```

For ordinary internet traffic this is essential.

For controlled low-latency local environments, the networking requirements can be very different.

---

# 41. Nagle's Algorithm

TCP may combine small writes into larger packets.

Suppose your program does:

```cpp
send("A")
send("B")
send("C")
```

Rather than sending three tiny TCP packets, TCP may wait and combine them:

```text
ABC
```

This improves bandwidth efficiency.

But it may introduce latency.

Low-latency applications often disable Nagle:

```cpp
setsockopt(
    fd,
    IPPROTO_TCP,
    TCP_NODELAY,
    ...
);
```

`TCP_NODELAY` means roughly:

> Send small TCP writes immediately rather than waiting to batch them through Nagle's algorithm.

Very common interview topic.

---

# 42. Buffering

Networking contains buffers everywhere.

```text
Application buffer
      ↓
socket send buffer
      ↓
kernel
      ↓
NIC TX queue
      ↓
switch queues
      ↓
NIC RX queue
      ↓
socket receive buffer
      ↓
application
```

Buffers help absorb bursts.

But buffers can also create latency.

---

# 43. Bufferbloat

Suppose a network queue becomes huge:

```text
packet
packet
packet
packet
packet
packet
packet
...
```

Instead of dropping packets, the network keeps buffering them.

Throughput might remain good, but latency becomes terrible.

This is sometimes called **bufferbloat**.

Low-latency systems generally care about keeping queues short.

---

# 44. Bandwidth vs Latency

These are different concepts.

## Bandwidth

How much data can be transferred per unit time.

Example:

```text
10 Gbps
```

## Latency

How long one message takes to travel.

Example:

```text
20 microseconds
```

You can have:

```text
very high bandwidth
but
high latency
```

or:

```text
low bandwidth
but
low latency
```

Trading systems care enormously about latency.

---

# 45. Throughput vs Latency

Throughput:

```text
messages / second
```

Latency:

```text
time / message
```

Optimizing throughput frequently involves batching:

```text
collect 100 messages
process all 100
```

This can improve throughput because overhead is amortized.

But the first message now waits for the batch.

So:

```text
batching
   ↓
higher throughput
potentially higher latency
```

This tradeoff appears everywhere in low-latency engineering.

---

# 46. Jitter

Average latency is not everything.

Suppose system A has:

```text
latency:
10 μs
10 μs
10 μs
10 μs
```

System B:

```text
5 μs
5 μs
5 μs
100 μs
```

System B may have a lower median but dangerous tail latency.

Variation in latency is called **jitter**.

Trading systems care heavily about:

```text
p50
p95
p99
p99.9
maximum latency
```

Predictability can be nearly as important as average speed.

---

# 47. Packet Serialization Delay

Even sending bits onto a cable takes time.

Suppose you have:

```text
1000-byte packet
1 Gbps network
```

1000 bytes:

```text
8000 bits
```

Serialization time:

```text
8000 / 1,000,000,000
≈ 8 μs
```

On a 10 Gbps network:

```text
≈ 0.8 μs
```

So faster links can reduce serialization delay in addition to providing more bandwidth.

---

# 48. Propagation Delay

Signals cannot travel instantly.

Fiber propagation is roughly on the order of:

```text
~200,000 km/s
```

So physical distance matters.

This is one reason trading firms colocate servers near exchanges.

Instead of:

```text
Trading server
     ↓
hundreds of miles
     ↓
Exchange
```

they place servers in or near the same data center:

```text
Trading server
    ↓
switch
    ↓
Exchange
```

---

# 49. Colocation

**Colocation** means placing your servers physically close to an exchange's infrastructure.

Benefits:

```text
less physical distance
fewer network hops
lower latency
more consistent latency
```

At very low latencies, even cable length can matter.

---

# 50. TCP vs UDP in Trading

A simplified model:

## Market Data

Often:

```text
UDP multicast
```

because you want:

```text
one sender
many receivers
low latency
```

with application-level handling of packet loss.

## Order Entry

Often:

```text
TCP
```

or another reliable protocol because losing an order is generally unacceptable.

Conceptually:

```text
Exchange

Market data:
Exchange ──UDP multicast──> trading firms

Orders:
Trading firm ──reliable connection──> Exchange
```

---

# 51. Why Not Use UDP for Everything?

Suppose you send:

```text
BUY 1,000 AAPL @ $200
```

and the UDP packet disappears.

That's a serious problem.

You could build reliability yourself, but then you are effectively reimplementing many transport-layer features.

For order entry, reliability often matters enough to justify a reliable protocol.

---

# 52. Packet Loss

Packets can be lost because of:

```text
switch queue overflow
NIC receive queue overflow
kernel buffer overflow
network congestion
hardware problems
application processing too slowly
```

Example:

```text
Packets arrive at 10 million/sec

Application handles 8 million/sec

queue fills
    ↓
packets dropped
```

Therefore low-latency systems must process packets quickly enough to avoid backlog.

---

# 53. Backpressure

Suppose:

```text
Producer = 10 million messages/sec
Consumer = 5 million messages/sec
```

The queue grows:

```text
producer
 ↓↓↓↓↓↓↓↓↓↓
[queue grows forever]
 ↓↓↓↓↓
consumer
```

Eventually:

- memory grows
- latency increases
- packets get dropped

You need some form of backpressure, dropping policy, or sufficient processing capacity.

---

# 54. Copying Costs

A normal packet path can involve memory copies.

Conceptually:

```text
NIC
 ↓
kernel memory
 ↓ copy
application memory
```

Copying data costs CPU cycles and memory bandwidth.

Low-latency systems try to use techniques such as:

```text
zero-copy
DMA
memory mapping
kernel bypass
preallocated buffers
```

to reduce these costs.

---

# 55. Zero-Copy

Zero-copy means avoiding unnecessary movement of data between memory buffers.

Instead of:

```text
NIC buffer
   ↓ copy
kernel buffer
   ↓ copy
user buffer
```

try to approach:

```text
NIC
 ↓
memory directly accessible by application
```

Fewer copies usually means:

```text
less CPU work
less memory bandwidth
lower latency
```

---

# 56. System Calls and Networking

Functions like:

```cpp
send()
recv()
```

normally enter the kernel.

Conceptually:

```text
user mode
   ↓
system call
   ↓
kernel mode
   ↓
networking code
   ↓
return
   ↓
user mode
```

System calls aren't enormously expensive individually, but at millions of messages per second, overhead accumulates.

This is another motivation for batching or kernel bypass.

---

# 57. Blocking vs Nonblocking Sockets

A blocking `recv()`:

```cpp
recv(fd, buffer, n, 0);
```

can put the thread to sleep until data arrives.

Conceptually:

```text
recv()
 ↓
no data
 ↓
thread sleeps
 ↓
packet arrives
 ↓
thread wakes
 ↓
recv returns
```

A nonblocking socket instead returns immediately.

```cpp
fcntl(fd, F_SETFL, O_NONBLOCK);
```

Then:

```cpp
recv(...)
```

may return something equivalent to:

```text
EAGAIN
```

meaning:

> No data is currently available.

---

# 58. select, poll, epoll

If you have many sockets, you do not want one thread per socket.

You can ask the OS which sockets are ready.

Older interfaces:

```text
select()
poll()
```

Linux:

```text
epoll()
```

Conceptually:

```text
Socket A ─┐
Socket B ─┼→ epoll → "B and D are ready"
Socket C ─┤
Socket D ─┘
```

Then your application only processes ready sockets.

---

# 59. epoll Mental Model

Example:

```cpp
epoll_wait(...)
```

blocks until one or more registered file descriptors become ready.

Then:

```text
epoll_wait()
     ↓
fd 5 readable
fd 9 readable
     ↓
recv(fd5)
recv(fd9)
```

This allows one thread to efficiently manage many connections.

---

# 60. Level-Triggered vs Edge-Triggered

`epoll` can operate in two major styles.

## Level Triggered

As long as data remains available:

```text
epoll keeps telling you:
"socket is readable"
```

## Edge Triggered

You are notified when the state changes:

```text
not readable
     ↓
data arrives
     ↓
readable
     ↓
notification
```

Typically you then drain the socket until:

```text
EAGAIN
```

Edge-triggered mode can reduce repeated notifications but is easier to misuse.

---

# 61. bind(), connect(), listen(), accept()

For TCP server:

```cpp
socket()
   ↓
bind()
   ↓
listen()
   ↓
accept()
```

For client:

```cpp
socket()
   ↓
connect()
```

Conceptually:

```text
Server                         Client

socket()                       socket()
bind()
listen()

                               connect()
                                  |
                                  |
accept() <------------------------+
```

`accept()` returns a **new socket** for that client.

The listening socket remains available for additional connections.

---

# 62. Four-Tuple

A TCP connection can be identified using:

```text
source IP
source port
destination IP
destination port
```

Example:

```text
10.0.0.5:50000
    →
10.0.0.10:443
```

So a server can have thousands of clients connected to the same server port because each connection has a different four-tuple.

---

# 63. Endianness

Networking protocols traditionally use **network byte order**, which is big-endian.

Suppose integer:

```text
0x12345678
```

Big-endian representation:

```text
12 34 56 78
```

Little-endian:

```text
78 56 34 12
```

Functions include:

```cpp
htonl()
htons()
ntohl()
ntohs()
```

Meaning:

```text
host to network long
host to network short
network to host long
network to host short
```

This is a classic systems interview topic.

---

# 64. MTU

MTU stands for **Maximum Transmission Unit**.

For standard Ethernet, commonly:

```text
1500 bytes
```

meaning approximately 1500 bytes of IP payload can fit inside a standard Ethernet frame.

If an IP packet is larger than the supported MTU, fragmentation may occur or the protocol/application may need to avoid generating oversized packets.

Low-latency systems generally prefer predictable packet sizes and avoid unnecessary fragmentation.

---

# 65. Jumbo Frames

Some networks support larger MTUs such as:

```text
9000 bytes
```

These are often called **jumbo frames**.

Advantages:

```text
more payload per packet
less per-packet overhead
better throughput
```

Potential disadvantage:

```text
larger serialization delay
```

Again:

```text
throughput optimization
vs
latency optimization
```

---

# 66. Checksums

Protocols may include checksums to detect corrupted packets.

Conceptually:

```text
sender:
data → checksum

receiver:
data → recompute checksum
         ↓
compare
```

If the values differ:

```text
packet corrupted
```

Ethernet also includes a frame check sequence.

---

# 67. DNS

DNS translates hostnames into IP addresses.

```text
exchange.example.com
        ↓
DNS
        ↓
192.0.2.20
```

Low-latency trading paths often avoid DNS lookups during critical operation.

You do not want something like:

```text
order arrives
    ↓
perform DNS request
    ↓
wait
    ↓
send order
```

Critical path operations should avoid unpredictable external work.

---

# 68. Important Latency Sources

When analyzing networking latency, think about:

```text
Application processing
        ↓
system call
        ↓
kernel networking
        ↓
queueing
        ↓
NIC processing
        ↓
serialization
        ↓
propagation
        ↓
switch processing
        ↓
destination NIC
        ↓
destination kernel
        ↓
destination application
```

Any of these can dominate depending on the system.

---

# 69. Typical Low-Latency Optimizations

Common ideas include:

```text
CPU pinning
NUMA awareness
busy polling
kernel bypass
zero-copy
preallocated buffers
avoid dynamic allocation
lock-free structures
short network paths
colocation
TCP_NODELAY
small queues
careful batching
RSS configuration
hardware timestamping
```

The general philosophy is:

> Remove unpredictable work from the critical path.

---

# 70. Interview Question: Why Is UDP Faster Than TCP?

Avoid simply saying:

> UDP is faster because its header is smaller.

That is incomplete.

A better answer:

> UDP has less protocol machinery. It does not maintain connection state, guarantee ordering, retransmit lost data, implement flow control, or perform TCP-style congestion control. This can reduce overhead and, importantly for low-latency applications, avoids waiting for retransmissions and ordered delivery. However, if reliability is required, the application may need to implement recovery itself.

---

# 71. Interview Question: Why Use UDP Multicast for Market Data?

Good answer:

> Exchanges need to distribute the same market data to many subscribers. UDP multicast allows the network to replicate one transmitted stream to many receivers instead of requiring the exchange to maintain a separate TCP stream for every subscriber. UDP also avoids head-of-line blocking. Because UDP is unreliable, market data protocols typically include sequence numbers and use gap-recovery or snapshot mechanisms.

---

# 72. Interview Question: What Happens When a Packet Arrives?

A strong answer:

```text
1. Ethernet frame reaches NIC.
2. NIC validates/processes the frame.
3. NIC places data into memory, usually using DMA.
4. NIC/kernel determines that data is available.
5. Network driver processes receive descriptors.
6. Ethernet header is processed.
7. IP layer processes the IP packet.
8. TCP/UDP layer processes the transport header.
9. Packet is associated with the correct socket.
10. Data becomes available to the application.
11. Application calls recv() or consumes it through another mechanism.
```

In low-latency systems, some of these steps can be bypassed or optimized.

---

# 73. Interview Question: What Happens When You Call send()?

Simplified:

```text
1. Application calls send().
2. CPU transitions into kernel mode.
3. Kernel finds the socket.
4. Data enters the networking stack.
5. TCP/UDP headers are created.
6. IP header is added.
7. Ethernet frame is created.
8. Data is placed in the NIC transmit queue.
9. NIC reads data using DMA.
10. NIC transmits bits onto the network.
```

The exact implementation can involve optimizations such as offloading and zero-copy mechanisms.

---

# 74. Interview Question: What Happens If UDP Packet #103 Is Missing?

Suppose:

```text
received:
100
101
102
104
```

Your application sees:

```text
expected = 103
received = 104
```

Therefore:

```text
gap detected
```

Possible strategy:

```text
1. Mark local market data state as potentially stale.
2. Buffer subsequent packets if appropriate.
3. Request missing packet(s) from recovery service.
4. Apply 103.
5. Apply buffered 104+.
6. Resume normal operation.
```

If the gap cannot be recovered, request a fresh snapshot.

---

# 75. Interview Question: Why Can TCP Be Bad for Market Data?

Suppose:

```text
100
101
102 ← lost
103
104
```

TCP guarantees ordered delivery.

Even if packets containing later bytes arrive:

```text
103
104
```

the application cannot consume those bytes ahead of the missing data.

It must wait for retransmission.

For rapidly changing market data, waiting can be worse than detecting the loss yourself and taking an application-specific recovery path.

---

# 76. Interview Question: Why Is Batching Good and Bad?

Batching:

```text
Packet
Packet
Packet
Packet
   ↓
process all together
```

Good:

```text
fewer system calls
better cache efficiency
amortized overhead
higher throughput
```

Bad:

```text
first packet waits for later packets
higher latency
potentially worse tail latency
```

For quant systems:

> You usually need to determine whether the system optimizes primarily for throughput or latency.

---

# 77. Interview Question: Why Does Cache Locality Matter for Networking?

Suppose every incoming packet updates an order book.

If the processing thread remains on one core:

```text
packet
 ↓
core 2
 ↓
order book already in L1/L2 cache
```

Fast.

If the thread constantly moves:

```text
core 2
 ↓
core 5
 ↓
core 1
```

the cache lines may need to move between cores.

This creates additional latency and cache-coherence traffic.

Therefore:

```text
CPU pinning
+
RSS configuration
+
NUMA placement
```

can matter significantly.

---

# 78. Trading System Networking Architecture

A simplified trading system might look like:

```text
                  EXCHANGE
                     |
         +-----------+-----------+
         |                       |
     Market Data              Order Entry
     UDP Multicast               TCP
         |                       ^
         v                       |
+------------------+      +----------------+
| Market Data Feed |      | Order Gateway  |
| Handler          |      +----------------+
+------------------+              ^
         |                        |
         v                        |
+------------------+              |
| Order Book       |              |
+------------------+              |
         |                        |
         v                        |
+------------------+              |
| Strategy         |--------------+
+------------------+
```

Critical path:

```text
market packet arrives
        ↓
decode packet
        ↓
update order book
        ↓
strategy reacts
        ↓
construct order
        ↓
send to exchange
```

Every nanosecond or microsecond along this path can matter.

---

# 79. Low-Latency Thinking

When someone asks:

> How would you optimize this networking system?

Think systematically:

```text
Are there unnecessary copies?

Are there unnecessary allocations?

Are we entering the kernel unnecessarily?

Are threads being scheduled?

Can threads be pinned?

Are packets processed on the correct NUMA node?

Is the NIC queueing packets?

Are network switches queueing packets?

Are we batching too aggressively?

Are locks required?

Can data remain in cache?

Are we suffering TCP head-of-line blocking?

Can multicast help distribution?

Can hardware offload work?

Can we poll instead of using interrupts?
```

---

# 80. Core Concepts to Memorize

For a quant developer interview, you should be very comfortable explaining:

```text
TCP vs UDP
TCP byte-stream semantics
UDP datagrams
TCP three-way handshake
TCP retransmission
TCP head-of-line blocking
sequence numbers
UDP multicast
market data gap detection
snapshot + incremental feeds
sockets
bind/connect/listen/accept
blocking vs nonblocking sockets
epoll
Ethernet
MAC vs IP vs port
switch vs router
ARP
NICs
DMA
interrupts
polling
ring buffers
RSS
CPU affinity
NUMA
kernel bypass
zero-copy
TCP_NODELAY
buffering
bandwidth vs latency
throughput vs latency
jitter/tail latency
MTU
endianness
```

---

# 81. Extremely Important Interview Mental Model

If you remember one pipeline, remember this:

```text
Exchange generates market update
        ↓
UDP multicast packet
        ↓
switch
        ↓
NIC
        ↓
DMA
        ↓
RX ring buffer
        ↓
CPU polls queue
        ↓
decode packet
        ↓
check sequence number
        ↓
update in-memory order book
        ↓
strategy evaluates state
        ↓
construct order
        ↓
send order over reliable session
        ↓
NIC
        ↓
exchange
```

Then think:

```text
Where could latency occur?

- network propagation
- serialization
- switching
- NIC
- interrupt/wakeup
- kernel
- copies
- cache misses
- locks
- allocations
- scheduling
- order-book processing
- order serialization
```

A large part of low-latency systems engineering is eliminating or reducing these costs.

---

# 82. One-Sentence Summary

> Low-latency networking is largely about moving packets from the NIC to application logic and back onto the wire with as little copying, queueing, scheduling, synchronization, and unpredictable work as possible while still maintaining the correctness guarantees the application requires.