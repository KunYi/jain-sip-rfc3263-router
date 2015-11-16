The following snippets show the log4j output from the router when using the [stateless proxy](http://code.google.com/p/jain-sip-rfc3263-router/source/browse/trunk/src/example/java/com/google/code/rfc3263/StatelessProxy.java) implementation from the example sources.  The first example shows the router attempting to find a route for `atlanta.com`:

```
0 [main] DEBUG com.google.code.rfc3263.DefaultRouter  - Router instantiated for gov.nist.javax.sip.SipStackImpl@4f80d6
2492 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - Attempting to route the following request 

REGISTER sip:atlanta.com SIP/2.0
Via: SIP/2.0/UDP 127.0.0.1:5062;rport=5062;branch=z9hG4bKnmefieyn;received=127.0.0.1
Max-Forwards: 70
To: "Alice" <sip:alice@atlanta.com>
From: "Alice" <sip:alice@atlanta.com>;tag=xbxwl
Call-ID: hjccavlujocnswe@127.0.0.1
CSeq: 1089 REGISTER
Contact: <sip:alice@127.0.0.1:5062>;expires=3600
Allow: INVITE,ACK,BYE,CANCEL,OPTIONS,PRACK,REFER,NOTIFY,SUBSCRIBE,INFO,MESSAGE
User-Agent: Twinkle/1.4.2
Content-Length: 0


2492 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - No route set found.  Using Request-URI for input
2498 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - Determining transports supported by stack
2498 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - Supported transports: [UDP, TCP]
2500 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Locating SIP server for sip:atlanta.com
2500 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Resolving TARGET for sip:atlanta.com
2501 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - TARGET is atlanta.com
2501 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isNumeric? atlanta.com
2501 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isIPv4Address? atlanta.com
2501 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isIPv4Address? atlanta.com: false
2501 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isIPv6Reference? atlanta.com
2502 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isIPv6Reference? atlanta.com: false
2502 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isNumeric? atlanta.com: false
2502 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Resolving TARGET for sip:atlanta.com
2502 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - TARGET is atlanta.com
2502 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Selecting transport for sip:atlanta.com
2502 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No transport parameter or port was specified.
2502 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Looking up NAPTR records for atlanta.com.
2619 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Supported NAPTR services: [SIP+D2T, SIP+D2U]
2619 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No NAPTR records found for atlanta.com.
2620 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Determining service identifier for UDP transport to atlanta.com.
2620 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Service identifier is _sip._udp.atlanta.com.
2620 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Looking up SRV records for _sip._udp.atlanta.com.
2651 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No valid SRV records for _sip._udp.atlanta.com.
2651 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Determining service identifier for TCP transport to atlanta.com.
2651 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Service identifier is _sip._tcp.atlanta.com.
2651 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Looking up SRV records for _sip._tcp.atlanta.com.
2677 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No valid SRV records for _sip._tcp.atlanta.com.
2677 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No SRV records found for atlanta.com.
2677 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Determining default transport for sip: scheme
2677 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Default transport is UDP
2677 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Transport selected for sip:atlanta.com: UDP
2677 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Determining IP address and port for sip:atlanta.com
2677 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No port is present in the URI
2678 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Determining default port for UDP
2678 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Default port is 5060
2706 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Hop list for sip:atlanta.com is [173.201.96.105:5060/UDP]
2708 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - Next hop for request is 173.201.96.105:5060/UDP
```

The following snippet shows the router attempting to find a route for `cisco.com` (which has some DNS support):

```
0 [main] DEBUG com.google.code.rfc3263.DefaultRouter  - Router instantiated for gov.nist.javax.sip.SipStackImpl@12cc95d
2821 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - Attempting to route the following request 

REGISTER sip:cisco.com SIP/2.0
Via: SIP/2.0/UDP 127.0.0.1:5062;rport=5062;branch=z9hG4bKgrjughak;received=127.0.0.1
Max-Forwards: 70
To: "Alice" <sip:alice@cisco.com>
From: "Alice" <sip:alice@cisco.com>;tag=vjjjb
Call-ID: hjccavlujocnswe@127.0.0.1
CSeq: 1083 REGISTER
Contact: <sip:alice@127.0.0.1:5062>;expires=3600
Allow: INVITE,ACK,BYE,CANCEL,OPTIONS,PRACK,REFER,NOTIFY,SUBSCRIBE,INFO,MESSAGE
User-Agent: Twinkle/1.4.2
Content-Length: 0


2823 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - No route set found.  Using Request-URI for input
2829 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - Determining transports supported by stack
2829 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - Supported transports: [UDP, TCP]
2831 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Locating SIP server for sip:cisco.com
2831 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Resolving TARGET for sip:cisco.com
2832 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - TARGET is cisco.com
2832 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isNumeric? cisco.com
2832 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isIPv4Address? cisco.com
2832 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isIPv4Address? cisco.com: false
2832 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isIPv6Reference? cisco.com
2833 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isIPv6Reference? cisco.com: false
2833 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - isNumeric? cisco.com: false
2833 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Resolving TARGET for sip:cisco.com
2833 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - TARGET is cisco.com
2833 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Selecting transport for sip:cisco.com
2833 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No transport parameter or port was specified.
2833 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Looking up NAPTR records for cisco.com.
2898 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Supported NAPTR services: [SIP+D2T, SIP+D2U]
2898 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No NAPTR records found for cisco.com.
2898 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Determining service identifier for UDP transport to cisco.com.
2898 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Service identifier is _sip._udp.cisco.com.
2898 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Looking up SRV records for _sip._udp.cisco.com.
2899 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No valid SRV records for _sip._udp.cisco.com.
2899 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Determining service identifier for TCP transport to cisco.com.
2899 [EventScannerThread] DEBUG com.google.code.rfc3263.util.LocatorUtils  - Service identifier is _sip._tcp.cisco.com.
2899 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Looking up SRV records for _sip._tcp.cisco.com.
3032 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Found 1 SRV record(s) for _sip._tcp.cisco.com.
3032 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Selecting service record from record set
3033 [EventScannerThread] DEBUG com.google.code.rfc3263.dns.ServiceRecordSelector  - Sorting service records by priority
3034 [EventScannerThread] DEBUG com.google.code.rfc3263.dns.ServiceRecordSelector  - Sorting service record(s) by weight for priority 1
3034 [EventScannerThread] DEBUG com.google.code.rfc3263.dns.ServiceRecordSelector  - Priority list has only one service record, no need to sort
3034 [EventScannerThread] DEBUG com.google.code.rfc3263.dns.ServiceRecordSelector  - Finished sorting service records
3035 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Transport selected for sip:cisco.com: TCP
3035 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Determining IP address and port for sip:cisco.com
3035 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - No port is present in the URI
3035 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - SRV records found during transport selection
3160 [EventScannerThread] DEBUG com.google.code.rfc3263.Locator  - Hop list for sip:cisco.com is [64.102.249.42:5060/TCP]
3162 [EventScannerThread] DEBUG com.google.code.rfc3263.DefaultRouter  - Next hop for request is 64.102.249.42:5060/TCP
```