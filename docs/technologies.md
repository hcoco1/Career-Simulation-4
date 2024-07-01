# Technologies

- **URL**: http://192.168.1.226
- **CMS**: WordPress
- **Analytics**: WordPress
- **Font Scripts**: Google Font API
- **Programming Languages**: PHP
- **Databases**: MySQL

The `http-wordpress-enum` script provided information about the WordPress plugins and themes installed on the target site. The results indicate the presence of the following:

## Plugins:
- Akismet
- Contact Form 7 (version 4.1)
- Jetpack (version 3.3.2)
- All-in-One SEO Pack
- Google Sitemap Generator (version 4.0.7.1)
- Google Analytics for WordPress (version 5.3.2)
- WPtouch (version 3.7.3)
- All-in-One WP Migration (version 2.0.4)
- WP Mail SMTP (version 0.9.5)

## Themes:
- Twentythirteen (version 1.6)
- Twentyfourteen (version 1.5)
- Twentyfifteen (version 1.3)

## Vulnerabilities

Nikto provided additional insights and potential security issues:

**SSL Info**:

  - Subject: /CN=www.example.com
  - Issuer: /CN=www.example.com
  - Ciphers: ECDHE-RSA-AES256-GCM-SHA384

**Server**:

  - Apache web server detected.

**Configuration Issues**:

  - Missing Strict-Transport-Security header.
  - Missing X-Content-Type-Options header.
  - Retrieved x-powered-by header: PHP/5.5.29.
  - Apache mod_negotiation enabled with MultiViews.
  - Content-Encoding header set to deflate, potentially vulnerable to BREACH attack.

**Interesting Directories/Files**:

  - /admin/
  - /image/
  - /wp-links-opml.php: Reveals the installed WordPress version.
  - /license.txt: May identify site software.
  - /admin/index.html: Admin login page/section found.
  - Multiple WordPress login pages (/wp-login.php, /wp-admin/wp-login.php, etc.)
  - wp-config.php file found, which contains credentials.



<div class="button-container" markdown="1">

<a href="/Career-Simulation-4/" class="md-button md-button--secondary">Home 🏠</a>


</div>