# Cloudflare
---

### Active scan
---
* Google Dorking:
  	>Google Dorking (also known as Google hacking) is a technique that leverages advanced search operators in Google Search to find specific information that is not easily accessible through regular search queries.

		using copyright text: "© 2024 Cloudflare, Inc."

*  Crawler:
  	>Crawler are used to systematically explore and gather information from websites and other online resources.
   	- Shodan CLI:
   	  >Command:
		```bash
		shodan search "hostname:<domain>"
   		```

* Subdomain Enumeration:
  	>Subdomain enumeration helps to identify subdomains—smaller, often hidden sections of a website or domain—that can reveal valuable information about a target, its infrastructure, and potential attack vectors.
   	- Linked and JS discovery:
   	  	- **Burp Suite**
			>file: burp.txt
		- **Hakrawler**
			>command:
			```bash
			echo "https://www.cloudflare.com" | hakrawler | sed 's|\(https\?://[^/]*\).*|\1|' | sort| uniq | grep -i "cloudflare"
   			```
			file: subd.txt

		- Subdomain scraping:
				**Google dork**
					methods: site:cloudflare.com -www.cloudflare.com

				**Subfinder**
					Command: subfinder -d cloudflare.com
					file: subf.txt

				**Shosubgo**
					file: shosub.txt

4. Service Scanning:
	
	Masscan:
		dnmasscan to convert domain name to ip
		and then scan for all port
		command: dnmasscan dom.txt dns.log -p80,443 -oG masscan.log

	nmap:
		command: nmap -sV 104.16.133.229 -oG nmap_op.txt 
