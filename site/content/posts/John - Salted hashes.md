---
tags: [cyber, hashcracking, john, outil]
title: John - Salted hashes
created: '2020-08-18T20:25:05.878Z'
modified: '2020-08-18T20:59:07.582Z'
---

# John - Salted hashes

https://github.com/openwall/john/issues/2125

`--format='dynamic=sha512($p.$s)`

then the input needs to be

`hash_in_hex`$`salt`

```sh
kali@kali:~/Documents$ sudo john --crack-status hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format='dynamic=sha512($p.$s)'
Using default input encoding: UTF-8
Loaded 1 password hash (dynamic=sha512($p.$s) [256/256 AVX2 4x])
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, almost any other key for status
november16       (?)
1g 0:00:00:00 DONE (2020-08-18 16:24) 100.0g/s 2016Kp/s 2016Kc/s 2016KC/s yasmeen..spongy
Use the "--show --format=dynamic=sha512($p.$s)" options to display all of the cracked passwords reliably
Session completed
```
