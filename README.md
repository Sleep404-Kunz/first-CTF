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
Checking connection to the target IP yields the following.

<img src= "https://github.com/Sleep404-Kunz/first-CTF/blob/main/webpage.png" alt = "Output" width = "400" />

### Step 2: NMAP scanning
- I used the NMAP tool to identify the OS, services and their versions running on the target.
- This allowed me to look for any vulnerabilities that the host services version might have that can be exploited to gain access to the target. 
  
```bash
nmap -sV -O 192.168.94.121
```
<img src= "https://github.com/Sleep404-Kunz/first-CTF/blob/main/NMAP%20output.png" alt = "Output" width = "700" />

The target machine is running two services:
- ssh (OpenSSH 8.4p1) on port 22 and
- http (Apache httpd 2.4.54) on port 80

### Step 3: Gobuster dir mode enumberation

I can then use the Gobuster scanning tool to find hidden URLS for the target ip. This process involved running the command numerous times with multiple wordlists. The wordlists available in kali by default were not successful in obtaining any successful results so, following a google search of popular wordlists I used the SecLists. A link is provided here: [Seclist link](https://github.com/danielmiessler/SecLists/tree/master)

The gobuster was successful wiht the CommonPHP-Filenames.txt wordlist and gave me the URL with /dashboard.php  that contained a credential for a ctfuser that will be used later on to gain initial access and foothold in the target. 

```bash
gobuster dir -u 192.168.94.121 -w /usr/share/wordlists/SecLists/Discovery/Web-Content/Common-PHP-Filenames.txt -b 404
```
<img src= "https://github.com/Sleep404-Kunz/first-CTF/blob/main/gobuster%20output.png" alt = "Output" width = "500" />

The new URL gives the webpage directing to the hint

<img src= "https://github.com/Sleep404-Kunz/first-CTF/blob/main/dashboard.png" alt = "Output" width = "400" />
<img src= "https://github.com/Sleep404-Kunz/first-CTF/blob/main/hint.png" alt = "Output" width = "550" />

There were other wordlists that provided results with hidden URLs, however those websites were not accessbile because of permission restrictions. 

### Step 4: SSH connection to target machine

I then attempted a SSH connection to the target IP address with the credentials obtained in step 4. Running the ls command shows two text files, one of which is the first flag. 

<img src= "https://github.com/Sleep404-Kunz/first-CTF/blob/main/SSH.png" alt = "Output" width = "550" />

### Step 5: Privilege escalation for root access

Since this CTF was meant to be for beginners like myself, I searched the internet for the most basic forms of privilege escaltion that did not involve running scripts or use of tools.

I check the ctfuser's permissions with the following command.

```bash
sudo -l
```
This result shows that ctfuser has root access without password authentication but only with vim. So, I chose to use the vim text editor exploit.

<img src= "https://github.com/Sleep404-Kunz/first-CTF/blob/main/ctfuser.png" alt = "Output" width = "550" />

To open the root shell, I create a new file with vim and I now open the vim command and open a root shell within the text editor.

```bash
ctfuser@e13d17876392:/$ sudo vim test
```
Within the vi session, which is already running with root privileges, I could now open a shell with the following command

```bash
:!/bin/bash
```

**🏁Lo and behold** I now have a shell with root privileges with access to the root home directories and the final flag.

<img src= "https://github.com/Sleep404-Kunz/first-CTF/blob/main/root flag.png" alt = "Output" width = "550" />


