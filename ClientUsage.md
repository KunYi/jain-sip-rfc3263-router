

The jain-sip-rfc3263-router library can be used in two ways: by specifying the DefaultRouter as the router to be used by JAIN-SIP, or by using the vendor's Router and manually using the location service to add a route set.

## Router ##

To use the jain-sip-rfc3263-router Router, simply set the `javax.sip.ROUTER_PATH` property to `"com.google.code.rfc3263.DefaultRouter"` in the Properties argument used to create a new SipStack:

```
Properties properties = new Properties();
properties.setProperty("javax.sip.ROUTER_PATH", "com.google.code.rfc3263.DefaultRouter");
properties.setProperty("javax.sip.STACK_NAME", "com.google.com.rfc3263");

SipFactory factory = SipFactory.getInstance();
SipStack stack = factory.createSipStack(properties);
```

The `javax.sip.USE_ROUTER_FOR_ALL_URIS` property **must** be absent or have a value of "true" for the router to be consulted.

### Potential Drawbacks ###

The router is **stateless**, so a request with an identical Request-URI (or top Route header where appropriate) will always be sent to the same server or group of servers in a constant DNS environment (subject to DNS caching).  This may cause problems if the SIP server recommended by DNS is invalid or temporarily unavailable, in which case you may choose to use the location service manually (see below) to specify an alternative route, should one exist.

### Non-SIP URIs ###

Non-SIP URIs, such as `tel:`, are **not supported** by the RFC3263 specification.  Applications which wish to route SIP and non-SIP URIs are invited to extend the router from this project as shown in [this example](http://code.google.com/p/jain-sip-rfc3263-router/source/browse/trunk/src/example/java/com/google/code/rfc3263/PstnRouter.java).

## Location Service ##

The first thing you should attempt is to retrieve a list of hops from the location service.  You create a new instance of Locator by passing in a list of preferred transports, in order of preference.  Here, we are stating that we prefer TCP to UDP.  We then attempt to find the next hops for `sip:bob@biloxi.com`:

```
SipFactory factory = SipFactory.getInstance();
AddressFactory addressFactory = factory.createAddressFactory();
SipURI uri = addressFactory.createSipURI("bob", "biloxi.com");

List<String> transports = new LinkedList<String>();
transports.add(ListeningPoint.TCP);
transports.add(ListeningPoint.UDP);

Locator locator = new Locator(transports);
Queue<Hop> hops = locator.locate(uri);
```

When attempting to send a request you should poll the queue for the most appropriate hop.  The hop should be used to create a new Route header like so:

```
HeaderFactory headerFactory = factory.createHeaderFactory();

SipURI routeUri = addressFactory.createSipURI(null, hop.getHost());
routeUri.setLrParam();
routeUri.setPort(hop.getPort());
if (hop.getTransport().equals("TLS")) {
    routeUri.setTransportParam("tcp");
} else if (hop.getTransport().equals("TLS-SCTP")) {
    routeUri.setTransportParam("sctp");
} else {
    routeUri.setTransportParam(hop.getTransport().toLowerCase());
}
Address routeAddress = addressFactory.createAddress(routeUri);
RouteHeader route = headerFactory.createRouteHeader(routeAddress);
```

You should then clone the request, add the route header to the top of route set, and send it using the most appropriate SipProvider.  For a clear division of security, it might be simpler to use two different providers, and assign secure ListeningPoint instances (TLS, SCTP-TLS) to one, and insecure ListeningPoint instances (UDP, TCP, SCTP) to the other.

```
Request clonedRequest = (Request) request.clone();
clonedRequest.addFirst(route);

SipProvider sendingProvider;
if (hop.getTransport().startsWith("TLS")) {
    sendingProvider = secureProvider;
} else {
    sendingProvider = insecureProvider;
}
```

Before sending, the request should have the selected Via header set too:

```
ListeningPoint endPoint = sendingProvider.getListeningPoint(hop.getTransport);
ViaHeader via = headerFactory.createViaHeader(endPoint.getIPAddress(), endPoint.getPort(), hop.getTransport(), null);
clonedRequest.addHeader(via);
ClientTransaction tx = sendingProvider.getNewClientTransaction(clonedRequest);
Dialog dialog = sendingProvider.getNewDialog(tx);
dialog.sendRequest(tx);
```

If the transaction fails (i.e. your listener receives a `IOExceptionEvent`), poll the next hop from the queue and try again.  When the queue is empty, no more hops could be determined, and message sending has failed.  Depending on the type of failure, you may choose to try some or all of the hops again.

Depending on the time between request-sending attempts, you may wish to invoke the `locate` method again, to see if the DNS has been updated for the domain you are attempting to contact.