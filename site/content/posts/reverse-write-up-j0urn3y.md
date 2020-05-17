---
title: "Reverse write-up : j0urn3y"
date: 2020-05-17T18:24:49.677Z
tags:
  - Reverse
  - Write-up
---
<!--StartFragment-->

For this first post, we are gonna dive into an easy reverse challenge, in other words, we have to find a flag by analysing a binary file. Let’s run it :

![](https://alecsis.netlify.app/images/uploads/1.png "First run")

A simple tool that can help finding some clues is called **strings**. It will read the binary file aiming for strings :

![](https://alecsis.netlify.app/images/uploads/2.png "Strings")

In the middle of the list, we find the strings that were displayed earlier, except for\*\*((Earth))\*\*that wasn’t. It may be the flag, let’s try it :

![](https://alecsis.netlify.app/images/uploads/3.png "((Earth)) guess")

It is not as simple, though. Let’s run **Ghidra** to get a high level overview of the code :

![](https://alecsis.netlify.app/images/uploads/5.png "Challenge")

![](https://alecsis.netlify.app/images/uploads/6.png "Lithosphere")

![](https://alecsis.netlify.app/images/uploads/7.png "Mantle")

![](https://alecsis.netlify.app/images/uploads/8.png "Core")

It appears that our guess goes through a lot of modifications and is eventually compared to the ((Earth)) string.

We also find that the string must be **9** characters long, in challenge.

In order to understand better what happens to our string, we input **123456789** in the program and we will monitor it’s state just before the **strcmp** call. We run **gdb-peda** to perform this dynamic diagnosis.

![](https://alecsis.netlify.app/images/uploads/9.png "Run gdb")

We first analyse the asm dump of the core function in order to find the location of strcmp :

![](https://alecsis.netlify.app/images/uploads/10.png "Disass core")

…

![](https://alecsis.netlify.app/images/uploads/11.png "Core strcmp")

Right above srtcmp are pushed the arguments 0x8048977 should be a pointer to ((Earth)) string. We verify it with the command `x/s0x8048977`, which reads in the specified memory location a string (s). We could also read a decimal (d) or hexa (x). The other argument pushed comes from eax, it is a special register used in almost every operations, it should point to our modified string.

![](https://alecsis.netlify.app/images/uploads/12.png "Good string")

As expected. Now, let’s break at core:+172 (at “push eax”). The code will stop at this location when the instruction pointer steps on it, and we will be able to read registers memory at that time.

![](https://alecsis.netlify.app/images/uploads/13.png "Breakpoint core")

The breakpoint hexa location doesn’t match perfectly the wanted location. It’s ok, we just have to input`ni`(next instruction and repeat enter) until we arrive to the target.

![](https://alecsis.netlify.app/images/uploads/14.png "Arrival at breakpoint")

As expected, the program breaks in a wrong location. Let’s `ni` a  few times until we step on push eax.

![](https://alecsis.netlify.app/images/uploads/15.png)

We observe that eax points to our modified string : **835472916**. We thus know the index offsets in order to get ((Earth)) after the modification. The good input must be **)r(aE)r(h**.

![](https://alecsis.netlify.app/images/uploads/16.png "Success")

***Well done !***

<!--EndFragment-->