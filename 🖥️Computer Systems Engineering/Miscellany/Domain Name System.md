The Domain Name System (DNS) is a protocol that maps human-readable ==domain names== (e.g. `eecs.mit.edu.`) into machine-usable ==IP addresses== (e.g. `18.25.0.23`). 

## Initial Attempts

Basic initial designs for DNS:

- **Centralized DNS server**. Every client queries the same server that holds the mapping. This is *hard to scale*.
- **Distributed file.** Every client holds its own copy of the mapping. This is *slow to update.

## DNS's Hierarchical and Distributed Design

The actual design of DNS is hierarchical and distributed. Your computer has a file `resolv.conf` which holds a couple of IP addresses of nameservers.

When your client wants to resolve a domain name, such as `eecs.mit.edu.`, it first queries your local DNS server (it got this by performing a name discovery broadcast: the Internet service provider assigns you an Internet address and also tells you the addresses of a few nameservers it operates; you could also hardcode one, such as Google's `8.8.8.8`).

Then, your local DNS server queries the root nameserver (cluster), which is in charge of the IP addresses of nameservers under `.`, such as `edu.` or `com.`. The root nameserver doesn't know the address of `eecs.mit.edu.` itself, but it can point you to the nameserver (cluster) that is in charge of `edu.` domain names.

Your local DNS server now recurses, and asks for `eecs.mit.edu.` from the `edu.` nameserver. It also doesn't know, but can point you to the `mit.edu.` nameserver. That one should give you the right response.

Now, every client and nameserver should store a local **cache** of recent resolutions, so you can just jump directly. These cached entries will each have their own expiry time, because you need updates (things could be broken temporarily). You can also do **negative caching** of common misspellings of domain names.