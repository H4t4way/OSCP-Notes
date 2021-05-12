# Enumeration


## Port Scanning :



```bash
nmap -sC -sV -o nmap 1 -A -T5 10.10.10.x

# Host Discovery
nmap -sn 10.10.1.1-254 -vv -oA hosts
netdiscover -r 10.10.10.0/24

# DNS server discovery
nmap -p 53 10.10.10.1-254 -vv -oA dcs

# NSE Scripts Scan
* nmap -sV --script=vulscan/vulscan.nse

# Port specific NSE script list :
ls /usr/share/nmap/scripts/ssh*
ls /usr/share/nmap/scripts/smb*

# Scanning all 65535 ports :

masscan -p1-65535,U:1-65535 --rate=1000 10.10.10.x -e tun0 > ports
ports=$(cat ports | awk -F " " '{print $4}' | awk -F "/" '{print $1}' | 
nmap -Pn -sV -sC -p$ports 10.10.10.x

# Running specific NSE scripts :
nmap -Pn -sC -sV --script=vuln*.nse -p$ports 10.10.10.x -T5 -A
```
sC - default scripts, sV - scan for versions, oA- output all formats. 

Optional - sT (performs full scan instead of syn-scan to prevent getting flagged by firewalls)
From Apache Version to finding Ubuntu version -> ubuntu httpd versions

## FTP : (Port 21)

anonymous login check
- ftp <ip address>
- username : anonymous
- pwd : anonymous
- file upload -> put shell.php


## SSH : (Port 22)



id_rsa.pub : Public key that can be used in authorized_keys for login

id_rsa : Private key that is used for login. Might ask for password. can be cracked with
ssh2john and john

- id_rsa
- ssh -i id_rsa user@10.10.10.x
- For passwordless login, add id_rsa.pub to target's authorized_keys
- ssh2john

# DNS Zone transfer check : (Port 53)


- If port 53 is open
- Add host to /etc/hosts
- dig axfr smasher.htb @10.10.10.135
- [https://ghostphisher.github.io/smasher2](https://ghostphisher.github.io/smasher2)
- Add the extracted domain to /etc/hosts and dig again


# RPC Bind (111)

 ```bash
rpcclient --user="" --command=enumprivs 1 -N 10.10.10.10
rpcinfo –p 10.10.10.10
rpcbind -p 10.10.10.10
```
