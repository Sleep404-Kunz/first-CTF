# First-CTF 📝

My first ever CTF challenge (ISC2). This was a level-easy CTV developed by members of the ISC2 Toronto Chapter. It was a really good hands on exercise to test my pentesting skills. This was also where I learned pentesting and hacking is more about researching the tools and how to use them and not just typing out commands in a terminal. The objective of the CTF was 2 layered, the first to get a foothold in the target and the second to gain access to files through prvilege escalation. 

- **CTF Name**: privisc2alation
- **Challenge Category**: Category like Web Exploitation
- **Challenge Difficulty**: Easy
- **Objective**: Identify vulnerable ports and gain initial foothold and collect flag 1. After gaining access execute privilege escalation to superuser to collect flag 2.


## 🔧 Tools used 

| Tool       | Purpose            |
|------------|--------------------|
| Kali VM    | Used as the attacking machine and also contains the docker instances for the CTF.                 |
| Docker     | Used to deploy the CTF challenge.                                                                 |
| NMAP       | Network scanning tool to identify the open ports in the target hosts.                  |
| Gobuster   | Enumerate hidden directories, URLs, and files  in the target domain.                   |
| nano       | Used as a means for privilege escalation.                   |


## 🚀 Process
### Step 1: Installing Docker and Docker Compose
```bash
apt install docker docker-compose
```
CTF file
```bash
version: '3.8'

services:
  privisc2alation:
    image:  eyeoncysec/privisc2alation:latest
    container_name: privisc2alation
    restart: unless-stopped
    networks:
      ctf-network:
        ipv4_address: 192.168.94.121
    stdin_open: true
    tty: true
networks:
  ctf-network:
    driver: bridge
    ipam:
      config:
        - subnet: 192.168.94.0/24apt install docker docker-compose
```
### Step 2: NMAP scanning
- I used the NMAP tool to identify the OS, services and their versions running on the host.
- This allowed me to look for any vulnerabilities that the host services version might have that can be exploited to gain access to the host. 
  
```bash
nmap -sV -O 192.168.94.121
```
Output

