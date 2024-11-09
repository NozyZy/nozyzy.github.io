---
title: "Wireshark/Tshark TCP illegal flags"
date: 2024-09-21T21:13:10+02:00
draft: false
tags: ['Forensics']
categories:
  - 'articles'

toc:
  enable: true
  auto: true
---

During the [patriotCTF](https://pctf.competitivecyber.club/), I had to filter all the illegal TCP packets from a network packet capture, to find how the flag was exfiltrated.

For that purpose, I based my knowledge of "illegal TCP packets" only on wrong flags combinations, displayed as red in wireshark:

{{< image src="/illegal_tcp_flags/img.png" caption="Wireshark red packets" title="Wireshark red packets" alt="Wireshark red packets" >}}

Here, only the combination of **FIN**, **SYN**, **RST** appears, with the addition of PSH sometimes.

However, I was missing something.

## TCP Flags

Here is a small recap of which TCP flags exist, and will be used for illegal combinations:

| Name        | Small form | Description                                                                                                                                                                                                                           |
|-------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Urgent      | URG        | Indicates that the Urgent pointer field is significant.                                                                                                                                                                               |
| Acknowledge | ACK        | Indicates that the Acknowledgment field is significant. All packets after the initial SYN packet sent by the client should have this flag set.                                                                                        |
| Push        | PSH        | Push function. Asks to push the buffered data to the receiving application.                                                                                                                                                           |
| Reset       | RST        | Reset the connection by non-genuinely dropping next packets.                                                                                                                                                                          |
| Synchronize | SYN        | Synchronize sequence numbers. Only the first packet sent from each end should have this flag set. Some other flags and fields change meaning based on this flag, and some are only valid when it is set, and others when it is clear. |
| Finish      | FIN        | Last packet from sender, end the connection genuinely.                                                                                                                                                                                |

[Source](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)

Each of these flags is set or not set using **one bit**, in the same order as described above, from the highest to the least significant bit.

In wirehsark, we can visualize those flags:
{{< image src="/illegal_tcp_flags/img_1.png" height="50%" caption="Flags visualisation" title="Flags visualisation" alt="Flags visualisation" >}}

A hex value is displayed to know which combination is set for each packet.
In this example, the flags RST, SYN and FIn are set, which can be written 000111 using the 6 flags described above, which is 0x07 in hexa.


## Illegal flags combination

Those flags, defined by [RFC9323](https://datatracker.ietf.org/doc/html/rfc9293) are meant to be used only under certain circumstances. It means that some combinations are allowed, where others are not.

For example, setting the **FIN** and **RST** flags together would mean to __reset__ the connection and __finish__ it genuinely. It is not coherent.

To find all the illegal combinations, I was looking for an already made tshark filter to easily find those illegal packets, and could not find it.

So I turned my research into finding the list of all illegal combinations, and found [that link](https://support.zyxel.eu/hc/en-us/articles/360001445493-Firewall-Abnormal-TCP-flag-attack-detected#tcp-flag-attacks-detected-in-firewall-1)

It is not official documentation because I was unable to interpret the actual whole RFC :)

The provided link lists a firewall rule set to detect illegal TCP flags, which could translate to an attack:
{{< image src="/illegal_tcp_flags/img_2.png" height="50%" caption="Some illegal combinations" title="Some illegal combinations" alt="Some illegal combinations" >}}

## Resulting filter

So I build this Wireshark/tshark filter, which reflects in the right order the previous screenshot:

{{< admonition success "Illegal TCP flags Filter" >}}
``tcp.flags == 0xfff || (tcp.flags.syn == True && tcp.flags.fin == True) || (tcp.flags.syn == True && tcp.flags.reset == True) || (tcp.flags.fin == True && tcp.flags.reset == True) || tcp.flags == 0x001 || tcp.flags == 0x008 || tcp.flags == 0x020``
{{< /admonition >}}

I was able to find the solution using those documentation and the resulting filter.

I hope it could help you (and myself too!) in the future :D
