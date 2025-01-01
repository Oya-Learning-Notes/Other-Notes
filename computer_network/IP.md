> This article is focus on IP protocol and its relevant info.

# IP Datagram

> CNPDFP-138

![IP Datagram](https://upload.wikimedia.org/wikipedia/commons/thumb/6/60/IPv4_Packet-en.svg/2560px-IPv4_Packet-en.svg.png)

In total, size of IP datagram should be n times of 4 bytes, let's call this a `DWORD`.

## Header

Header is **usually 20 bytes**. (5 DWORD) There is a 4-bit size to record the total length of header. Which means the **maximum size of header is $2^4 - 1$ DWORD (15 DWORD, 60 bytes)**

Need paddings if not multiple of DWORD.

## Fragments

- `DF`: Don't fragment.
- `MF`: More fragment.

When to fragmentation?
- When **packages size > MTU of lower layer** (Data link layer, MAC frame maybe)

Things to remember:

- Same group of fragmentation should share the same `Identification` part.
- `Fragment Offset` 
	- Use 2 DWORD as unit. (Thus, size of data part of fragmentation is always multiple of 2 DWORD)
	- Only consider the offset of data, do not taking headers into consideration.
- When an IP package is fragmented, we need to set `MF:=1` flag for non-final fragments.

## Data

Total Length (16-31, 2byte, max=2^16-1=65535), with `bytes` as unit. Which indicates one single IP datagram could have maximum length of 65535 bytes (header + data)

Fragment offset, use `64bit` as unit. Which means all length of **data part** of fragment should be n-times of 64bit (8 bytes)

Need paddings if not multiple of DWORD.

## TTL

Set `TTL:=1` to limit the package inside a LAN*(Local Access Network)*

## Unit Conclusion

- IHL: `DWORD(32bit)`
- Fragment Offset: `2*DWORD`
- Total Length: `Byte` (Maybe choose smaller unit will convenient the padding removal process?)

# CIDR

> CNPDFP-151

Abandon terms of category A, B, C and D network. Unified Subnet representation format into:

```
IP = NetworkId + HostId
```

We could use slash notation to represent the number of bits of the network id:

```
  10110100.10010110.10000010.00101110 / 20
->10110100.10010110.1000                 (Subnet ID)
->                      0010.00101110    (Host ID)
```

## More Granularity

Think about the old, categorized IP allocation method, which the IANA could only choose to allocate one of the three type of IP as a whole:

- A: Contains maximum $2^{24}-2$ hosts. (16,777,214)
- B: Contains maximum $2^{16}-2$ hosts. (65,534)
- C: Contains maximum $2^{12}-2$ hosts. (4,096)

So, what if I want about 1,000,000 addresses? Which one should I apply for, 16 million is too many, but 65k is definitely not enough. Lack of more granularity, right?

With CIDR, I could apply for some sub network like `x.x.x.x/20` network, which contains $2^{20}=1,048,576$ hosts.


## How CIDR Alleviate Routing Table Size

To be simply, merge items in routing table if we could/we want.

![CIDR Allocation Example](https://github.com/user-attachments/assets/869b4f8b-608e-44b7-a8b7-2d9a65a431d4)

Consider a CIDR allocation at the example above.

- ISP get a `/18` subnet.
- ISP allocate a `/22` to a university.
- University allocate `/23` `/24` and `/25` subnet to its own internal branches.

In a traditional way, this will cause the **exchange of all subnet info**(like the University, Major1, Major2, ...) when some other routers communicate with the ISP router.

However, we know they somehow have an inclusive relation, which is the `206.0.68.0/18`(ISP subnet) actually contains all the networks (and all subnet) of the university. So, it's theoretically feasible to merge those items in the Routing Table when exchanged.

| Addr | Subnet Mask | Interface |
|---|---|---|
|206.0.68.0|/18|R_i|

This essentially means that, for other routers, whenever an IP falls into this CIDR range `206.0.68.0/18`, give it to router `R_i` _(should be some router owned by ISP)_

> Note:
> 
> The ISP router and university router should still keep the items of those subnet if they need to have the ability to directly deliver packages on that network.

## Maximum Prefix Matches

Easy to understand, when more than one items matched in the routing table, choose the one with long matches in subnet-id part.

Keep using example above, if a router `R_a` connected to `R_i`(ISP router) and `R_u`(University router) simultaneously, then it would have:

| Addr | Subnet Mask | Interface |
|---|---|---|
|206.0.64.0|/18|R_i|
|206.0.68.0|/22|R_u|

Consider if at this time, the router received a package with IP `206.0.68.1`, then it will **match both subnet of these two items**.

However, it's quite obvious it would be a better choice that we directly pass the package to `R_u`, the university router. In general, it could be proved that we should always choose the item with longer subnet-id match (Or, with longer CIDR subnet mask length).

> This optimal match could be implemented using Binary Trie. 
> CNPDFP-156

# ICMP

> Encapsulated in IP package data.

`PING` (Packet InterNet Groper) and `TraceRoute` are based on `ICMP` protocol. And this is an example of Application Layer directly use Network Layer (Skip the Transmissive Layer like TCP and UDP)

## TraceRoute

```
Tracing route to baidu.com [110.242.68.66]
over a maximum of 30 hops:

  1    <1 ms    <1 ms    <1 ms  192.168.1.1
  2     1 ms     1 ms     1 ms  172.29.0.1
  3     *        *        *     Request timed out.
  4     2 ms     2 ms     2 ms  120.193.78.17
  5     1 ms     *        1 ms  112.31.241.117
  6     *        *        *     Request timed out.
  7     *        *        *     Request timed out.
  8     *        *        *     Request timed out.
  9    23 ms    23 ms     *     219.158.33.157
 10     *        *        *     Request timed out.
 11     *        *        *     Request timed out.
 12    26 ms    26 ms    27 ms  110.242.66.190
 13    26 ms    26 ms    26 ms  221.194.45.130
 14     *        *        *     Request timed out.
 15     *        *        *     Request timed out.
 16     *        *        *     Request timed out.
 17    26 ms    26 ms    26 ms  110.242.68.66
```

Trace routes exploit the `TTL` settings in IP protocol. The working mechanism is sending packages with different TTL and observe the source IP addr of the ICMP timeout exception package.

> CNPDFP-159

