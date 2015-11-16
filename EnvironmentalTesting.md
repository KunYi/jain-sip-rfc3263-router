Testing the router in different DNS environments is critical to ensuring reliability.  The environment is defined by the presence and configuration of four different records:

  * NAPTR
  * SRV
  * A
  * AAAA

Of the above, A and AAAA have equivalent meaning, since the results are grouped.

## NAPTR ##

Example:

```
IN NAPTR 100 10 "S" "SIP+D2U" "" _sip._udp.example.com.
IN NAPTR 102 10 "S" "SIP+D2T" "" _sip._tcp.example.com.
```

Routing results are affected by the _order_ (low to high) and _preference_ (low to high) of NAPTR records.

## SRV ##

Example:

```
_sip._tcp.example.com. 86400 IN SRV 0 5 5060 primary.example.com.
_sip._udp.example.com. 86400 IN SRV 1 5 5060 backup.example.com.
```

SRV records are sorted primarily by _priority_ (low to high).  2782 states that records with equal _priority_ field values should then select a record using a weighted random function with reference to the _weight_ field.

However, to ensure that stateless proxies always retrieve the same record, 3263 changes the selection algorithm so that lower weights are used first, and there is no random element.  Since records of equal _priority_ and _weight_ may be present, we further sort by the _target_ field.

## Existing Test Environments ##

The router is currently tested in the following environments:

| **A** | **S** | **N** |
|:------|:------|:------|
| <font color='red'>✘</font> | <font color='red'>✘</font> | <font color='red'>✘</font> |
| <font color='green'>✔</font> | <font color='red'>✘</font> | <font color='red'>✘</font> |
| <font color='green'>✔</font> | <font color='green'>✔</font> | <font color='red'>✘</font> |
| <font color='green'>✔</font> | <font color='green'>✔</font> | <font color='green'>✔</font> |

### Key ###

  * A = A or AAAA records
  * S = SRV records
  * N = NAPTR records