# NAT - Development of a multi-threaded NAT

This is a course project under IK2200 of KTH. The whole project is based on FastClick which is a extended Click Router Module. Thanks for the instructions from Tom Barbette and my teammates Yuan Fan and Lida Liu. If you want to know more about FastClick, you can go to Barbette’s GitHub. [FastClick](https://github.com/tbarbette/fastclick)

This repo is mainly focus on the NAT performance analysis. There are two stages one is for single core NAT while another is for multi-core. 

## Task 1 - Develop a single core NAT

The configuration of single-core NAT is based on Click language and is run with FastClick. In order to fulfil the NAT, we need to realize functions by using Click Elements. The following Figure is a flowchart to show how the configuration finishes the NAT process.

![Flowchart of Single-core NAT configuration]()

First, when the NAT machine received a packet, it needs to distinguish the type of the packet. If the packet is an ARP request, NAT will generate the response and send it to the corresponding interface. If the packet is an ARP response, then NAT will encapsulate ethernet header found via ARP into IP packets. If the packet is an IP packet, NAT will classify its type (TCP, UDP or ICMP) and send the packet into corresponding Rewriter Elements.

It is really similar for TCPRewriter, UDPRewriter, ICMPRewriter, and ICMPPingRewriter elements. The parameter that they use, is from IPRewriterPatterns, which specify the new source IP address and source port. After all of these finished, NAT will broadcast the ARP query to request the Ethernet address of destination machine and send out the rewritten packet.

Since there is little information about how to make a NAT through click language, you can have a look on my code or maybe modify it to fit your requirements. For this script, it  needs DPDK supported which means you need to bind your NICs with DPDK driver.

Here is the script for [Single core NAT](). I also add the ICMP rewriting function into the NAT.

## Task 2 - Develop a multi-thread NAT

### Per-core duplication, with software classification

Really similar to the previous task, but what we need to add is to using CPUSwitch element in order to duplicate the process of NAT function. When a packet comes back, we need to send it to a corresponding Rewrite flow table based on its destination port.

You can see the whole configuration in 2-core-NAT.click and 4-core-NAT.click. I will update the explanation for the script.

To be continued....

## Data Testing and Analysis

All the data we get is from Trex, which is a open source packet generation tool developed by Cisco. Here is the GitHub for [Trex](https://github.com/cisco-system-traffic-generator).

Also, we have written a python to generate graph. You can find it in the Data Analysis folder. For more issues, see Wiki in this repo.

---

This repo is only used for research. If you have any question, please e-mail me.
