wildcard-dns
============

A tiny single threaded regular expression (wildcard) matching DNS server


##Uses
This server is useful when you need to extract the answer (IP) from the question (Domain).

For example, you might need to provide a valid SSL cert covered domain for every IP that you have, and 
doing so becomes very easy with this server.

##Installation
Requires Python

Make sure the script is executable by running:
```bash
chmod +x wildcard-dns
```

##Usage
Run:
```bash
./wildcard-dns BINDADDR PATTERN TTL
```

where

- **BINDADDR**: 
  
  Address to bind to, such as `212.59.18.25` or `dns.myserver.com`.

  Use `0.0.0.0` or `""` to bind to all addresses available on this machine.
  
- **PATTERN**:
  
  Wildcard pattern with `[A]`.`[B]`.`[C]`.`[D]` tokens of the IP,
  as well as optional `[IGNORE]` token which matches anything.
  
Pattern | Domain name (Question) | Resolves to (Answer)
--- | --- | ---
`[A]`-`[B]`-`[C]`-`[D]`.ip.test.com | 1-2-3-4.ip.test.com | 1.2.3.4
`[D]`-`[A]`-`[B]`-`[C]`.ip.test.com | 1-2-3-4.ip.test.com | 2.3.4.1
`[A]`-`[B]`-`[C]`-`[D]`.`[IGNORE]`.test.com | 1-2-3-4.XX.test.com | 1.2.3.4
`[A]`-`[B]`-`[C]`-`[D]`.`[IGNORE]`.test.com | 1-2-3-4.YY.test.com | 1.2.3.4

  Please note that tokens should be **UPPERCASE** and surrounded by [ and ].

- **TTL**:

  Time-to-live in seconds. 
  
  Other DNS servers will cache the answer for this given period.
  Increase this value to reduce the amount of requests coming through to your server.
  
  Maximum value is 4294967295 seconds (~136 years)
