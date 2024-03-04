You have a client and a server that want to talk to each other. Of these two ==endpoints==, the client is the ==source==, and the server is the ==destination==. 

How is this possible?

* **Point-to-point links** (ethernet cables, Bluetooth, nearby wireless). This is unscalable: you would need to maintain $\binom{n}{2}$ connections between every pair of machines.
* **Switches** (routers, gateways). These help forward data to destinations that are far away. (They also do other things.)

This allows us to model the network as a graph:

![[network.png|center|384]]

We can partition the internet functionalities into a few ==layers==:

- **Link protocols:** handle communication between two directly-connected nodes.
- **Network protocols:** handle naming, addressing, and ==efficient routing== of packets, which are a couple thousand bytes of data, tagged with metadata in the header.
- **Transport protocols:** ensure reliable sharing of the network when it scales to many clients.
- **Applications:** the things that actually generate traffic on the network. What people want to use the internet for.

> [!idea]
> With this layered structure, we should be able to swap out protocols at one layer without much (perhaps any) change to protocols in other layers. 

^76e3cd

For example, it doesn't matter if you use ethernet or WiFi to watch cat videos. Swapping the link layer technology should not affect everything else. Swapping out transport protocols should also be supported!

---

**Next:** [[History of the Internet]]









