# **Step 2: Network Scanning**
!!! note ""

- Run a basic scan to check the Network:

```python linenums="1" hl_lines="11"
┌──(hcoco1㉿kali)-[~]
└─$ ip addr                                                       
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:39:98:6e brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.223/24 brd 192.168.1.255 scope global dynamic noprefixroute eth0
       valid_lft 84939sec preferred_lft 84939sec
    inet6 2600:1700:1580:4b60::26/128 scope global dynamic noprefixroute 
       valid_lft 2142sec preferred_lft 2142sec
    inet6 2600:1700:1580:4b60:8291:bf25:fefb:aca5/64 scope global temporary dynamic 
       valid_lft 3514sec preferred_lft 3514sec
    inet6 2600:1700:1580:4b60:a00:27ff:fe39:986e/64 scope global dynamic mngtmpaddr noprefixroute 
       valid_lft 3514sec preferred_lft 3514sec
    inet6 fe80::a00:27ff:fe39:986e/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever

```

  >Based on the output  provided from  `ip addr` command, we can see that the Kali Linux machine is configured with the IP address `192.168.1.223` on the `eth0` interface, and it's within the `/24` subnet. 


- Identify which hosts are up in the `192.168.1.0/24` subnet, you can start with a ping sweep using Nmap. This is a non-intrusive way to discover active hosts.

```python linenums="1" hl_lines="28"
┌──(hcoco1㉿kali)-[~]
└─$ nmap -sn 192.168.1.0/24           
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-25 05:24 EDT
Nmap scan report for amazon-7543b6401.attlocal.net (192.168.1.67)
Host is up (0.16s latency).
Nmap scan report for 192.168.1.71
Host is up (0.011s latency).
Nmap scan report for 192.168.1.75
Host is up (0.012s latency).
Nmap scan report for amazon-d79ebfdc6.attlocal.net (192.168.1.77)
Host is up (0.12s latency).
Nmap scan report for RokuExpress.attlocal.net (192.168.1.81)
Host is up (0.0092s latency).
Nmap scan report for wlan0.attlocal.net (192.168.1.84)
Host is up (0.0084s latency).
Nmap scan report for wlan0.attlocal.net (192.168.1.85)
Host is up (0.0075s latency).
Nmap scan report for unknown1009f9067e58.attlocal.net (192.168.1.86)
Host is up (0.12s latency).
Nmap scan report for 192.168.1.196
Host is up (0.00090s latency).
Nmap scan report for unknownf2a4540f4500.attlocal.net (192.168.1.197)
Host is up (0.00086s latency).
Nmap scan report for Redmi-Note-12-Pro-5G.attlocal.net (192.168.1.221)
Host is up (0.0088s latency).
Nmap scan report for 192.168.1.223
Host is up (0.00058s latency).
Nmap scan report for 192.168.1.226
Host is up (0.00067s latency).
Nmap scan report for dsldevice.attlocal.net (192.168.1.254)
Host is up (0.047s latency).
Nmap done: 256 IP addresses (14 hosts up) scanned in 7.85 seconds

```

>Now that you have identified the target VM, you can perform more in-depth scan on the IP addresses. The goal here is to discover which services are running on the target VM and to identify any potential vulnerabilities associated with these services. 

- Next, run service and version detection scans on the specific IP addresses found in your first scan. Scan for services beginning at port 1 and ending at port 5000.

```python linenums="1" hl_lines="11"
┌──(kali㉿kali)-[~]
└─$ 
nmap -sV -p1-5000 192.168.1.226

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-25 05:40 EDT
Nmap scan report for unknownf2a4540f4500.attlocal.net (192.168.1.226)
Host is up (0.0072s latency).
Not shown: 4997 filtered tcp ports (no-response)
PORT    STATE  SERVICE    VERSION
22/tcp  closed ssh
80/tcp  open   tcpwrapped
443/tcp open   tcpwrapped

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.72 seconds

```

The Nmap scan results show that most ports are filtered (no-response), and the only open ports are 80 (http) and 443 (https), but they are reported as tcpwrapped, and port 22 (ssh) is closed.

'tcpwrapped' generally means that the service running on the port is protected by TCP wrappers and the server is not providing any information about the service. This can be due to security measures that limit the visibility of services to unauthorized scanners.

To gather more information, we can try the following:

Use the --reason option to understand why ports are reported as they are.

```python linenums="1" hl_lines="10"
┌──(hcoco1㉿kali)-[~]
└─$ sudo nmap -sV -p1-5000 --reason 192.168.1.226

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-25 05:59 EDT
Nmap scan report for unknownf2a4540f4500.attlocal.net (192.168.1.226)
Host is up, received arp-response (0.00078s latency).
Not shown: 4997 filtered tcp ports (no-response)
PORT    STATE  SERVICE  REASON         VERSION
22/tcp  closed ssh      reset ttl 64
80/tcp  open   http     syn-ack ttl 64 Apache httpd
443/tcp open   ssl/http syn-ack ttl 64 Apache httpd
MAC Address: 08:00:27:52:3B:4C (Oracle VirtualBox virtual NIC)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 34.92 seconds

```

The updated Nmap scan results indicate that ports 80 and 443 are open and running Apache HTTPD, while port 22 (SSH) is closed. This suggests that the web server on the target machine might be a potential entry point for further investigation.











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