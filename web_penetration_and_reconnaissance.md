# **Web Penetration/Reconnaissance**
## **General Tips & Tricks and Info**
- Looking at source code often comes in handy (CTRL+U)
- Viewing the robots.txt found in all webservers to view hidden files
  - For web reconnaissance, robots.txt serves as a valuable source of intelligence. While respecting the directives outlined in this file, security professionals can glean crucial insights into the structure and potential vulnerabilities of a target website:
    - Uncovering Hidden Directories: Disallowed paths in robots.txt often point to directories or files the website owner intentionally wants to keep out of reach from search engine crawlers. These hidden areas might house sensitive information, backup files, administrative panels, or other resources that could interest an attacker.
    - Mapping Website Structure: By analyzing the allowed and disallowed paths, security professionals can create a rudimentary map of the website's structure. This can reveal sections that are not linked from the main navigation, potentially leading to undiscovered pages or functionalities.
    - Detecting Crawler Traps: Some websites intentionally include "honeypot" directories in robots.txt to lure malicious bots. Identifying such traps can provide insights into the target's security awareness and defensive measures.
- Virtual Hosts
  - virtual hosting is the ability of web servers to distinguish between multiple websites or applications sharing the same IP address. This is achieved by leveraging the HTTP Host header, a piece of information included in every HTTP request sent by a web browser
  - In essence, the Host header functions as a switch, enabling the web server to dynamically determine which website to serve based on the domain name requested by the browser.
  - The key difference between VHosts and subdomains is their relationship to the Domain Name System (DNS) and the web server's configuration.
    - Subdomains: These are extensions of a main domain name (e.g., blog.example.com is a subdomain of example.com). Subdomains typically have their own DNS records, pointing to either the same IP address as the main domain or a different one. They can be used to organize different sections or services of a website.
    - Virtual Hosts (VHosts): Virtual hosts are configurations within a web server that allow multiple websites or applications to be hosted on a single server. They can be associated with top-level domains (e.g., example.com) or subdomains (e.g., dev.example.com). Each virtual host can have its own separate configuration, enabling precise control over how requests are handled
## **Web Reconnaissance**
- **Primary Goals**
  - Identifying Assets: Uncovering all publicly accessible components of the target, such as web pages, subdomains, IP addresses, and technologies used. This step provides a comprehensive overview of the target's online presence.
  - Discovering Hidden Information: Locating sensitive information that might be inadvertently exposed, including backup files, configuration files, or internal documentation. These findings can reveal valuable insights and potential entry points for attacks.
  - Analysing the Attack Surface: Examining the target's attack surface to identify potential vulnerabilities and weaknesses. This involves assessing the technologies used, configurations, and possible entry points for exploitation.
  - Gathering Intelligence: Collecting information that can be leveraged for further exploitation or social engineering attacks. This includes identifying key personnel, email addresses, or patterns of behaviour that could be exploited.
- Web reconnaissance encompasses two fundamental methodologies: active and passive reconnaissance
  - Active reconnaissance provides a direct and often more comprehensive view of the target's infrastructure and security posture. However, it also carries a higher risk of detection, as the interactions with the target can trigger alerts or raise suspicion.
  - Passive reconnaissance is generally considered stealthier and less likely to trigger alarms than active reconnaissance. However, it may yield less comprehensive information, as it relies on what's already publicly accessible.
- **Automating** **Recon**
  - Reconnaissance Frameworks
    - These frameworks aim to provide a complete suite of tools for web reconnaissance:
      - [FinalRecon](https://github.com/thewhiteh4t/FinalRecon): A Python-based reconnaissance tool offering a range of modules for different tasks like SSL certificate checking, Whois information gathering, header analysis, and crawling. Its modular structure enables easy customisation for specific needs.
      - [Recon-ng](https://github.com/lanmaster53/recon-ng): A powerful framework written in Python that offers a modular structure with various modules for different reconnaissance tasks. It can perform DNS enumeration, subdomain discovery, port scanning, web crawling, and even exploit known vulnerabilities.
      - [theHarvester](https://github.com/laramies/theHarvester): Specifically designed for gathering email addresses, subdomains, hosts, employee names, open ports, and banners from different public sources like search engines, PGP key servers, and the SHODAN database. It is a command-line tool written in Python.
      - [SpiderFoot](https://github.com/smicallef/spiderfoot): An open-source intelligence automation tool that integrates with various data sources to collect information about a target, including IP addresses, domain names, email addresses, and social media profiles. It can perform DNS lookups, web crawling, port scanning, and more.
      - [OSINT Framework](https://osintframework.com/): A collection of various tools and resources for open-source intelligence gathering. It covers a wide range of information sources, including social media, search engines, public records, and more.
- **Wayback Machine**
  - The Wayback Machine is a treasure trove for web reconnaissance, offering information that can be instrumental in various scenarios. Its significance lies in its ability to unveil a website's past, providing valuable insights that may not be readily apparent in its current state:
    1. Uncovering Hidden Assets and Vulnerabilities: The Wayback Machine allows you to discover old web pages, directories, files, or subdomains that might not be accessible on the current website, potentially exposing sensitive information or security flaws.
    1. Tracking Changes and Identifying Patterns: By comparing historical snapshots, you can observe how the website has evolved, revealing changes in structure, content, technologies, and potential vulnerabilities.
    1. Gathering Intelligence: Archived content can be a valuable source of OSINT, providing insights into the target's past activities, marketing strategies, employees, and technology choices.
    1. Stealthy Reconnaissance: Accessing archived snapshots is a passive activity that doesn't directly interact with the target's infrastructure, making it a less detectable way to gather information.
- **Search Operators**
  - Search operators are like search engines' secret codes. These special commands and modifiers unlock a new level of precision and control, allowing you to pinpoint specific types of information amidst the vastness of the indexed web. While the exact syntax may vary slightly between search engines, the underlying principles remain consistent.
  - ![image-20240919-111931.png](Aspose.Words.4f53b4a8-c711-4616-b410-f3f0a257863e.001.png) 
  - Google Dorking, also known as Google Hacking, is a technique that leverages the power of search operators to uncover sensitive information, security vulnerabilities, or hidden content on websites, using Google Search. A Few example shown below
    - Finding Login Pages:
      - site:example.com inurl:login
      - site:example.com (inurl:login OR inurl:admin)
    - Identifying Exposed Files:
      - site:example.com filetype:pdf
      - site:example.com (filetype:xls OR filetype:docx)
    - Uncovering Configuration Files:
      - site:example.com inurl:config.php
      - site:example.com (ext:conf OR ext:cnf) (searches for extensions commonly used for configuration files)
    - Locating Database Backups:
      - site:example.com inurl:backup
      - site:example.com filetype:sql
    - More examples: [Google Dorking](https://www.exploit-db.com/google-hacking-database)
- **Well known URIs**
  - The .well-known standard, defined in [RFC 8615](https://datatracker.ietf.org/doc/html/rfc8615), serves as a standardized directory within a website's root domain. This designated location, typically accessible via the /.well-known/ path on a web server, centralizes a website's critical metadata, including configuration files and information related to its services, protocols, and security mechanisms.
  - The Internet Assigned Numbers Authority (IANA) maintains a [registry](https://www.iana.org/assignments/well-known-uris/well-known-uris.xhtml) of .well-known URIs, each serving a specific purpose defined by various specifications and standards.
  - In web recon, the .well-known URIs can be invaluable for discovering endpoints and configuration details that can be further tested during a penetration test. One particularly useful URI is openid-configuration.

    The openid-configuration URI is part of the OpenID Connect Discovery protocol, an identity layer built on top of the OAuth 2.0 protocol. When a client application wants to use OpenID Connect for authentication, it can retrieve the OpenID Connect Provider's configuration by accessing the https://example.com/.well-known/openid-configuration endpoint. This endpoint returns a JSON document containing metadata about the provider's endpoints, supported authentication methods, token issuance, and more:

    {

    `  `"issuer": "https://example.com",

    `  `"authorization\_endpoint": "https://example.com/oauth2/authorize",

    `  `"token\_endpoint": "https://example.com/oauth2/token",

    `  `"userinfo\_endpoint": "https://example.com/oauth2/userinfo",

    `  `"jwks\_uri": "https://example.com/oauth2/jwks",

    `  `"response\_types\_supported": ["code", "token", "id\_token"],

    `  `"subject\_types\_supported": ["public"],

    `  `"id\_token\_signing\_alg\_values\_supported": ["RS256"],

    `  `"scopes\_supported": ["openid", "profile", "email"]

    }

    The information obtained from the openid-configuration endpoint provides multiple exploration opportunities:

    1. Endpoint Discovery:
       1. Authorization Endpoint: Identifying the URL for user authorization requests.
       1. Token Endpoint: Finding the URL where tokens are issued.
       1. Userinfo Endpoint: Locating the endpoint that provides user information.
    1. JWKS URI: The jwks\_uri reveals the JSON Web Key Set (JWKS), detailing the cryptographic keys used by the server.
    1. Supported Scopes and Response Types: Understanding which scopes and response types are supported helps in mapping out the functionality and limitations of the OpenID Connect implementation.
    1. Algorithm Details: Information about supported signing algorithms can be crucial for understanding the security measures in place.
## **Graphical Web Proxies**
The biggest benefit of Graphical Web Proxies is they allow for access to a combination command line tools all in a single app

Burp Suite & ZAP are two notable web proxies, however Burp Suite is licensed leaving many enviable features behind a hefty paywall

Notable feature -

- Complete history of web requests / responses
- Altering response/request to send unexpected input to the server
- Automatic Modification of certain headers
- Repeating Requests
- Encoding/Decoding Text
- Web Fuzzers & Scanners
- Interception of web requests from command-line tools and other applications
- Active Scanner < Burp suite pro / ZAP >
  - The Burp Active scanner is considered one of the best tools in that field and is frequently updated to scan for newly identified web vulnerabilities by the Burp research team
  - It starts by running a Crawl and a web fuzzer (like dirbuster/ffuf) to identify all possible pages
  - It runs a Passive Scan on all identified pages
  - It checks each of the identified vulnerabilities from the Passive Scan and sends requests to verify them
  - It performs a JavaScript analysis to identify further potential vulnerabilities
  - It fuzzes various identified insertion points and parameters to look for common vulnerabilities like XSS, Command Injection, SQL Injection, and other common web vulnerabilities
  - It creates a confidence vs severity vulnerability assessment report while showing proof-of-concept details of how to exploit the vulnerability and information on how to remediate it
## **Command Line Tools** 
- **Gobuster >** <https://github.com/OJ/gobuster>
  - A tool used to brute-force URIs (directories and files) in web sites, DNS subdomains, Virtual Host names on target web servers, and more
  - Useful commands
    - gobuster dir -u http://<target>/ -w <wordlist>
      - /usr/share/wordlists/dirbuster
    - gobuster dns -d <target> -w /usr/share/SecLists/Discovery/DNS/namelist.txt
      - Used to enumerate subdomains
      - Prereqs
        - git clone <https://github.com/danielmiessler/SecLists>
        - sudo apt install seclists -y
- **cURL >** <https://everything.curl.dev/>
  - An extremely tool powerful in the right hands, used to transfer data to or from a server
  - Useful commands
    - curl -X [GET/POST/PUT/DELETE] <target>
      - Sends specifically crafted web requests
    - curl -IL <target>
      - Gets banner of target
- **whatweb >** <https://github.com/urbanadventurer/WhatWeb>
  - A useful tool with over 1800 plugins used to identify version numbers, email addresses, account IDs, web framework modules, SQL errors, and more on a website
  - whatweb <target>
- **Proxychains** > <https://github.com/haad/proxychains>
  - a tool that forces any TCP connection made by any given application to follow through proxy, useful for covering your tracks
  - proxychains nmap -sT -PO -p 80 -iR (find some webservers through proxy)
- **ffuf > <https://github.com/ffuf/ffuf>**
  - The term fuzzing refers to a testing technique that sends various types of user input to a certain interface to study how it would react. This is done because web servers do not usually provide a directory of all available links and domains (unless terribly configured), and so we would have to check for various links and see which ones return pages which would take a while manually
  - ffuf -w <WORDLIST> -u http://SERVER\_IP:PORT/FUZZ
