# Summary ☠️

This report outlines a structured methodology for ethical hacking and securing systems. It specifically targets a vulnerable Vulnhub Virtual Machine called Mr. Robot, which contains a WordPress site.

The process begins by setting up a controlled, isolated environment to ensure safe testing without impacting live systems. Network scanning with tools like Nmap is used to identify live hosts, open ports, and available services, creating a detailed map of the target network.
The next phase involves enumeration, where tools such as Gobuster and Wappalyzer are employed to brute-force directories and identify technologies used by the target WordPress site. This information is crucial for understanding the target's infrastructure. Next, vulnerabilities are identified using Nmap scripts and Nikto, which scan for outdated software like old versions of WordPress and potential security issues such as SQL injection or cross-site scripting in web servers.

Brute-force attacks use Python scripts and Hydra to guess login credentials, exposing weak passwords and access points. Finally, the guide explains how to achieve a reverse shell using a PHP script or Metasploit, allowing control over the target system. This understanding and control over the system, gained through the penetration testing, instills a sense of confidence and empowerment in the tester.

In conclusion, the guide underlines the importance of ethical hacking and responsible disclosure. It stresses that penetration testing is vital for improving security and maintaining secure systems when conducted ethically and with responsible disclosure. Responsible disclosure is a guideline and a commitment to the community and protecting the systems we test. This commitment makes the work of a penetration tester genuinely impactful.


## <a href="/Career-Simulation-4/technologies" ">Key Technologies</a>


The target system utilizes WordPress as its CMS and analytics platform. Key technologies include Google Font API for font scripts, PHP for programming, and MySQL for databases. The http-wordpress-enum script revealed several plugins and themes in use, including Akismet, Contact Form 7 (version 4.1), Jetpack (version 3.3.2), All-in-One SEO Pack, Google Sitemap Generator (version 4.0.7.1), Google Analytics for WordPress (version 5.3.2), WPtouch (version 3.7.3), All-in-One WP Migration (version 2.0.4), and WP Mail SMTP (version 0.9.5). The themes identified were Twentythirteen (version 1.6), Twentyfourteen (version 1.5), and Twentyfifteen (version 1.3).

## <a href="/Career-Simulation-4/technologies" ">Vulnerabilities</a>

Additional insights from Nikto indicated several potential security issues, such as missing Strict-Transport-Security and X-Content-Type-Options headers and an outdated PHP version (PHP/5.5.29). Apache mod_negotiation is enabled with MultiViews, and the Content-Encoding header is set to deflate, which may be vulnerable to a BREACH attack. Interesting directories and files found include /admin/, /image/, /wp-links-opml.php, /license.txt, and multiple WordPress login pages (/wp-login.php, /wp-admin/wp-login.php). A wp-config.php file containing credentials was also discovered. Using a reverse shell, three hidden keys were found, and the username "elliot" credentials were identified.
Findings

## Findings

>The penetration test identified several critical and high-severity vulnerabilities within the target system. The most severe issues include open ports running Apache HTTPD (CVSS Score 9) and retrieved credentials (CVSS Score 9), necessitating immediate updates and access control reviews. 

>Additionally, the test found high-severity SSL configuration issues (CVSS Score 8) and outdated PHP versions (CVSS Score 8), which require updates to security headers and server configurations. Medium and low-severity findings included outdated WordPress themes and plugins (CVSS Scores 5 and 6) and exposed sensitive directories/files (CVSS Score 5), all of which should be regularly updated and secured.

| Finding | CVSS Score | Severity | Finding Name                 | Description                                                        | Recommendation                                               |
| ------- | ---------- | -------- | ---------------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------ |
| 4       | 5          | Medium   | WordPress Themes             | Multiple themes including outdated versions                        | Update themes and remove unused ones                         |
| 7       | 5          | Low      | Directories/Files            | Various sensitive directories and files exposed                    | Restrict access to sensitive directories and files           |
| 3       | 6          | Medium   | WordPress Plugins            | Various plugins including outdated versions                        | Regularly update all plugins and monitor for vulnerabilities |
| 2       | 8          | High     | SSL Info                     | Missing security headers and outdated SSL configurations           | Update SSL/TLS settings and add security headers             |
| 5       | 8          | High     | Configuration Issues         | Missing security headers and outdated PHP version                  | Update server configurations and PHP version                 |
| 1       | 9          | High     | HTTP (80/tcp) - Apache HTTPD | Open ports 80 and 443 running Apache HTTPD, potential entry points | Ensure Apache is up-to-date and configure security headers   |
| 6       | 9          | Critical | Credentials Found            | Username and password retrieved (elliot/ER28-0652)                 | Change all passwords and review user access controls         |




| CVSS Base Score | CVSS Severity Level |
| --------------- | ------------------- |
| 9.0 - 10.0      | Critical            |
| 7.0 - 8.9       | High                |
| 0.1 - 3.9       | Low                 |
| 4.0 - 6.9       | Medium              |
| 0               | None                |


### **Hidden keys**:

- Flag 1 of 3: **073403c8a58a1f80d943455fb30724b9**  

- Flag 2 of 3: **822c73956184f694993bede3eb39f959**

- Flag 3 of 3: **04787ddef27c3dee1ee161b21670b4e4**
