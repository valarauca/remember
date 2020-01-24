nc
---

**N**et **C**at

[manual page](https://linux.die.net/man/1/nc)

There are at least 3 major versions of this tool:

1. `BSD`
2. `GNU`
3. `Apple`

They all use different command line interfaces, but some
options are similiar. 

If you just want a tool to throw packets at something
netcat is pretty good. But if you want it to work on a 
diverse range of end points (espeically cross OS) maybe
use something else.

## -l flag

Listen flag. This will set up a listener

```sh
nc -l 1234
```

For example will open a TCP listener on port `1234`

## send traffic

Simple way to send TCP traffic:

```sh
nc "${IP}:${PORT}"
```

What ever it gets from `stdin` will be send over the network

## -4 flag

This flag means it'll only use IPv4

## -6 flag

This flag means it'll only use IPv6

## -s flag

This flag lets you spoof your IP source, is an error to use with `-`l


