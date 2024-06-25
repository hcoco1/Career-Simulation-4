# **Step 4: Vulnerabilities**
!!! note ""

## nmap

- Use  nmap to enumerate WordPress-specific information, such as installed plugins, themes, and versions.

```python linenums="1" hl_lines="38 44"
┌──(hcoco1㉿kali)-[~]
└─$ sudo nmap -p80,443 --script http-wordpress-enum 192.168.1.226

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-25 07:07 EDT
Nmap scan report for unknownf2a4540f4500.attlocal.net (192.168.1.226)
Host is up (0.0022s latency).

PORT    STATE SERVICE
80/tcp  open  http
| http-wordpress-enum: 
| Search limited to top 100 themes/plugins
|   plugins
|     akismet
|     contact-form-7 4.1
|     jetpack 3.3.2
|     all-in-one-seo-pack 
|     google-sitemap-generator 4.0.7.1
|     google-analytics-for-wordpress 5.3.2
|     wptouch 3.7.3
|     all-in-one-wp-migration 2.0.4
|     wp-mail-smtp 0.9.5
|   themes
|     twentythirteen 1.6
|     twentyfourteen 1.5
|_    twentyfifteen 1.3
443/tcp open  https
| http-wordpress-enum: 
| Search limited to top 100 themes/plugins
|   plugins
|     akismet
|     contact-form-7 4.1
|     jetpack 3.3.2
|     all-in-one-seo-pack 
|     google-sitemap-generator 4.0.7.1
|     google-analytics-for-wordpress 5.3.2
|     wptouch 3.7.3
|     all-in-one-wp-migration 2.0.4
|     wp-mail-smtp 0.9.5
|   themes
|     twentythirteen 1.6
|     twentyfourteen 1.5
|_    twentyfifteen 1.3
MAC Address: 08:00:27:52:3B:4C (Oracle VirtualBox virtual NIC)

Nmap done: 1 IP address (1 host up) scanned in 5.13 seconds

```
### Findings


The `http-wordpress-enum` script provided information about the WordPress plugins and themes installed on the target site. The results indicate the presence of the following:

Plugins:

- akismet
- contact-form-7 (version 4.1)
- jetpack (version 3.3.2)
- all-in-one-seo-pack
- google-sitemap-generator (version 4.0.7.1)
- google-analytics-for-wordpress (version 5.3.2)
- wptouch (version 3.7.3)
- all-in-one-wp-migration (version 2.0.4)
- wp-mail-smtp (version 0.9.5)

Themes:

- twentythirteen (version 1.6)
- twentyfourteen (version 1.5)
- twentyfifteen (version 1.3)

>This enumeration provides a list of potential attack vectors as these plugins and themes might have known vulnerabilities that could be exploited.


## nikto

- Use nikto against the identified directories and files to check for known vulnerabilities.

```bash linenums="1" hl_lines="38 44"
┌──(hcoco1㉿kali)-[~]
└─$ nikto -h https://192.168.1.226

- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          192.168.1.226
+ Target Hostname:    192.168.1.226
+ Target Port:        443
---------------------------------------------------------------------------
+ SSL Info:        Subject:  /CN=www.example.com
                   Ciphers:  ECDHE-RSA-AES256-GCM-SHA384
                   Issuer:   /CN=www.example.com
+ Start Time:         2024-06-25 07:13:05 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache
+ /: The site uses TLS and the Strict-Transport-Security HTTP header is not defined. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Strict-Transport-Security
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ /UlQw0ECg.conf: Retrieved x-powered-by header: PHP/5.5.29.
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ /index: Uncommon header 'tcn' found, with contents: list.
+ /index: Apache mod_negotiation is enabled with MultiViews, which allows attackers to easily brute force file names. The following alternatives for 'index' were found: index.html, index.php. See: http://www.wisec.it/sectou.php?id=4698ebdc59d15,https://exchange.xforce.ibmcloud.com/vulnerabilities/8275
+ Hostname '192.168.1.226' does not match certificate's names: www.example.com. See: https://cwe.mitre.org/data/definitions/297.html
+ /: The Content-Encoding header is set to "deflate" which may mean that the server is vulnerable to the BREACH attack. See: http://breachattack.com/
+ /admin/: This might be interesting.
+ /image/: Drupal Link header found with value: <https://192.168.1.226/?p=23>; rel=shortlink. See: https://www.drupal.org/
+ /wp-links-opml.php: This WordPress script reveals the installed version.
+ /license.txt: License file found may identify site software.
+ /admin/index.html: Admin login page/section found.
+ /wp-login/: Cookie wordpress_test_cookie created without the secure flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
+ /wp-login/: Cookie wordpress_test_cookie created without the httponly flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
+ /wp-login/: Admin login page/section found.
+ /wordpress/: A Wordpress installation was found.
+ /wp-admin/wp-login.php: Wordpress login found.
+ /wordpress/wp-admin/wp-login.php: Wordpress login found.
+ /blog/wp-login.php: Wordpress login found.
+ /wp-login.php: Wordpress login found.
+ /wordpress/wp-login.php: Wordpress login found.
+ /#wp-config.php#: #wp-config.php# file found. This file contains the credentials.
+ 8102 requests: 0 error(s) and 22 item(s) reported on remote host
+ End Time:           2024-06-25 07:17:20 (GMT-4) (255 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

```


### Findings

Nikto provided additional insights and potential security issues:

SSL Info:

- Subject: /CN=www.example.com
- Issuer: /CN=www.example.com
- Ciphers: ECDHE-RSA-AES256-GCM-SHA384

Server:

- Apache web server detected.

Configuration Issues:

- Missing Strict-Transport-Security header.
- Missing X-Content-Type-Options header.
- Retrieved x-powered-by header: PHP/5.5.29.
- Apache mod_negotiation enabled with MultiViews.
- Content-Encoding header set to deflate, potentially vulnerable to BREACH attack.

Interesting Directories/Files:

- /admin/
- /image/
- /wp-links-opml.php: Reveals the installed WordPress version.
- /license.txt: May identify site software.
- /admin/index.html: Admin login page/section found.
- Multiple WordPress login pages (/wp-login.php, /wp-admin/wp-login.php, etc.)
- wp-config.php file found, which contains credentials.











<div id="disqus_thread"></div>
<script>
    /**
    *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
    *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables    */
    /*
    var disqus_config = function () {
    this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
    this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() { // DON'T EDIT BELOW THIS LINE
    var d = document, s = d.createElement('script');
    s.src = 'https://hcoco1-1.disqus.com/embed.js';
    s.setAttribute('data-timestamp', +new Date());
    (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

!!! note ""

<div class="button-container" markdown="1">
<a href="/Career-Simulation-3/challenge_1/" class="md-button md-button--primary">Previous: Challenge 1</a>
<a href="/Career-Simulation-3/" class="md-button md-button--secondary">Home 🏠</a>
<a href="/Career-Simulation-3/challenge_3/" class="md-button md-button--primary">Next: Challenge 3</a>
</div>