This walkthrough shows how the router would be applied to the example "Session Setup" messages from 3261.  The following message has just arrived at Alice's proxy (atlanta.com), which is using only UDP.

```
INVITE sip:bob@biloxi.com SIP/2.0
Via: SIP/2.0/UDP pc33.atlanta.com;branch=z9hG4bKnashds8
Max-Forwards: 70
To: Bob <sip:bob@biloxi.com>
From: Alice <sip:alice@atlanta.com>;tag=1928301774
Call-ID: a84b4c76e66710
CSeq: 314159 INVITE
Contact: <sip:alice@pc33.atlanta.com>
Content-Type: application/sdp
Content-Length: 142
```

## Determining the URI ##

The router examines the message, finds no `Route` headers, and thus determines that the correct URI to route for is the request URI, `sip:bob@biloxi.com`.

## Determining the Transport ##

There is no transport parameter or port configured, so the first step will be to determine what transports biloxi.com supports.

The router does a DNS query for NAPTR records for biloxi.com, and the nameserver returns the following:

```
biloxi.com. 86400 IN NAPTR 100 10 "S" "SIP+D2U" "" _sip._udp.biloxi.com.
biloxi.com. 86400 IN NAPTR 100 10 "S" "SIP+D2T" "" _sip._tcp.biloxi.com.
```

The second record is discarded, as TCP is not supported, which leaves UDP.  The router then performs a DNS query for the SRV record mentioned in the NAPTR record above:

```
_sip._udp.biloxi.com. 86400 IN SRV 0 5 5060 server10.biloxi.com.
_sip._udp.biloxi.com. 86400 IN SRV 1 5 5060 server11.biloxi.com.
```

server10 has a lower priority field that server11 (0 vs 1), so that record is selected first.  Now the router knows the transport (UDP), the port (5060) and the server (server10.biloxi.com.).

The router will now lookup A and AAAA records for server10.biloxi.com to generate a collection of hops, the first of which is used as the route.  When using the locator (instead of the router), both server10 and server11 are selected, and their corresponding A and AAAA records are added to the hop collection in the order stated.