The internet has become so crucial in modern life that internet shutdowns have become a powerful tool for oppression. Here is a brief history.

## ARPANET

The U.S. Advanced Research Projects Agency Network in the 1970s. It started with two machines:

![[arpanet2.png|center|256]]

The early machines looked like this:

![[arpanetfirstmachine.png|center|128]]

Then, the network gradually grew and added more machines.

![[arpanet8.png|center|256]]

Find MIT in the image below!

![[arpanetlarge.png|center|512]]


## Flexibility and Layering

Recall the [[Introduction to Networking#^76e3cd|layered model]] from our introduction.

The main network protocol by 1978 is called IP, and the main transport protocols are called TCP and UDP. TCP gives some reliability, but UDP is faster.

![[arpanetgeo.png|center|512]]

## Growth and Problems

In the 1980s, increased growth has started to cause problems. Queues are full so packets keep getting dropped and resent, perpetuating the issue. This was called the ==congestion collapse==. To fix this, congestion control was added to TCP.

Another desired technique was called ==policy routing==, which supports routing packets dynamically based on the endpoints, since maybe you don't want to allow certain actors to use parts of your network.

Another issue was distribution of IP addresses. In the past, organizations would be given contiguous chunks of addresses for efficiency of routing. ==Classless Inter-Domain Routing (CIDR)== solved this issue by giving out addresses is a different way without sacrificing efficiency. This involves changes to details about how switches route packets. This was possible because all switches were managed by CISCO, and forwarding was done in software (both untrue today!).

![[voxinternet.png|center|512]]

## Commercialization

By this time, the internet was entirely a commercial phenomenon. Changes largely stopped, even though there were many ideas about proposed enhancements. 

==Network Address Translation (NAT)== was one idea: multiple private addresses in a local network all get mapped to the same public IP address for the rest of the world to interact with. Deployment is nontrivial, since now the protocols need to support it.

## Modern Internet

A lot of the issues we face in the modern internet come from the internet's history of insecure, incremental design. This is because they were created before these issues existed.

For example, DDOS attacks are possible for this reason.

Another example is mobilization: nobody expected us to have phones in our pocket that are highly mobile yet connect to the internet from all variety of locations.

Here is a picture of the modern internet. Each dot is not a single machine, but rather its own large network (e.g. the size of MIT's).

![[moderninternet.png|center|512]]