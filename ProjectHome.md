

This project extends the capabilities of [JAIN-SIP](https://jain-sip.dev.java.net/) by using DNS procedures detailed in [RFC 3263](http://www.ietf.org/rfc/rfc3263.txt) to enable a UAC to determine the IP address (**including IPv6**), port and transport protocol of the next hop for a SIP request.

## Why should I care about using DNS in SIP? ##

Firstly, using DNS to determine the next hop allows you to understand the capabilities of the SIP entity to which you're routing a request.  RFC 3263 provides a mechanism for a SIP server to advertise what transports it supports, and in what preference.  So if you need to route to a server that only supports TLS, you can do that **without any local configuration**.

Secondly, RFC 3263 provides the ability for a server to identify secondary hosts, for **failover** and **load-balancing** purposes.  So if the server you're trying to contact suddenly fails, you can pick a new one with no loss of service.

## How do I use it? ##

Adding DNS support to your JAIN-SIP user agent is extremely simple, and often requires changing only **one line of code**.  To use the router from this project in your JAIN-SIP application, add the latest JAR to your application's classpath, and create a new `SipStack` like so:
```
Properties properties = new Properties();
properties.setProperty("javax.sip.ROUTER_PATH", "com.google.code.rfc3263.DefaultRouter");

SipFactory sipFactory = SipFactory.getInstance();
SipStack sipStack = sipFactory.createSipStack(properties);
```

For more information, please visit the [usage](ClientUsage.md) page.

## ENUM Support ##

If you want ENUM support too, consider looking at the [JAIN-SIP Extension](http://code.google.com/p/jain-sip.ext) project.