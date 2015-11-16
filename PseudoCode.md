This page is primarily for development purposes.  It summarises RFC 3263 as pseudo-code.

## Selecting a Target ##

```
if (uri has maddr parameter)
    target = maddr parameter
else
    target = uri.host-part
```

## Selecting a Transport ##

```
if (uri has transport parameter)
    transport =  uri.transport
else
    if (target is numeric)
        if (uri is sips)
            transport = TLS
        else
            transport = UDP
    else
        if (uri has port)
            if (uri is sips)
                transport = TLS
            else
                transport = UDP
        else
            if (naptr records exist for target)
                if (srv records exist for naptr record)
                    transport = srv.transport
                else
                    if (uri is sips)
                        transport = TLS
                    else
                        transport = UDP
            else
                if (srv records exist for target)
                    transport = srv.transport
                else
                    if (uri is sips)
                        transport = TLS
                    else
                        transport = UDP
```

## Selecting a Host ##

```
if (target is numeric)
    host = target
else
    if (uri has port)
        host = address from A record for target
    else
        if (uri has transport)
            if (srv records exist for target)
                host = address from A record for srv.host
            else
                host = address from A record for target
        else
            if (naptr records exist for target)
                if (srv records exist for naptr record)
                    host = address from A record for srv.host
                else
                    host = address from A record for target
            else
                if (srv records exist for target)
                    host = address from A record for srv.host
                else
                    host = address from A record for target
```

## Selecting a Port ##

```
if (target is numeric)
    if (uri has port)
        port = uri.port
    else
        port = default for transport
else
    if (uri has port)
        port = uri.port
    else
        if (uri has transport)
            if (srv records exist for target)
                port = srv.port
            else
                port = default for transport
        else
            if (naptr records exist for target)
                if (srv records exist for naptr record)
                    port = srv.port
                else
                    port = default for transport
            else
                if (srv records exist for target)
                    port = srv.port
                else
                    port = default for transport
```