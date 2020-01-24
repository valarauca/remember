openssl s_client
---

`openssl s_client` is a useful utility
for diagnosis SSL/TLS security and handshake
issues.

Generally speaking the command is in the format
of

```sh
openssl s_client -connect "${HOST}:${PORT}"
```

[manual page](https://www.openssl.org/docs/man1.0.2/man1/s_client.html)

## Protocol Flags

`s_client` attempts to be flexiable when connecting
so it may select any supported protocol you can
assert which protocol it should be via

```
-ssl2
-ssl3
-tls1
-tls1_1
-tls1_2
-tls1_3
-nossl2
-nossl3
-notls1
-notls1_2
-notls1_3
```

To provide restrictions & requirements on what to use.

## General Usage

To just dump a session handshake:

```sh
openssl s_client \
    -connect "${HOST}:${PORT}" \
    -nossl2 -nossl3 \
    -debug -state -nbio
```

This will display a lot of useful information about the handshake.

## Other uses

You can provide CA information, session keys, filter ciphers, and
curves if wished. See the manual page for more information.
