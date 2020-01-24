lsof
---

**L**i**S** of **O**pen **F**iles

[manual page](http://man7.org/linux/man-pages/man8/lsof.8.html)

Useful because everything in Unix (Linux) is a file,
or should be, kind of, its complicated, not really.

Most the time you'll use this to view network connections.

## View Active (TCP) Listeners

```sh
lsof -n -iTCP -sTCP:LISTEN
```

This will generate a list of all active listening
sockets on a local host.

## i flag

The `-i` flag offers many options to filter handles
by their semantics

```enbf
version = "4" | "6"
protocol = "TCP" | "UDP"
host = [ "hostname" | "ipv4" | "ipv6" ] // I'm not going to write a regex to validate them.
service = [ "service_name" | "port" ] // service name is magic don't rely on it

-i[version][protocol][@host][:service]
```

## s flag

The `-s` flag means "size" which is why we use it
to filter by connection state. This makes no sense,
unix tools are hilarious contradictions of usefulness
and purpose.

Generally speaking you _should_ use this with `-i`

```ebnf
tcp_state = "CLOSED" | "IDLE" | "BOUND" | "LISTEN" |
            "ESTABLISHED" | "SYN_SENT" | "SYN_RCDV" |
            "CLOSE_WAIT" | "FIN_WAIT1" | "CLOSING" |
            "LAST_ACK" | "FIN_WAIT_2" | "TIME_WAIT"
udp_state = "UNBOUND" | "IDLE"
tcp = "TCP" [:tcp_state]
udp = "UDP" [:udp_state]

-s[tcp|udp]
```

