# Cloudflare
---
### Passive Recon

---

* Acquisitions:
  	>Acquisitions of a company refer to the businesses, assets, or companies that a particular company has purchased or taken control. Acquisitions provides a holistic understanding of a company’s strategies, market positioning, risks, and potential for growth.
	- **Crunchbase**
   		>Crunchbase is a comprehensive platform and database that provides information about businesses, startups, investors, and industry trends. It is widely used by entrepreneurs, investors, researchers, and analysts to gather data on companies, track funding rounds, discover new startups, and understand the overall startup ecosystem.<br>
     	>File: [acquisitions.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/passive/acquisitions.txt)	

* ASN Enumeration:
  	>An ASN (Autonomous System Number) is a unique identifier assigned to an Autonomous System (AS) in the context of BGP (Border Gateway Protocol) routing on the internet.

	- **BGP view api** <br>
		> command:
	  ```bash
	  curl 'https://api.bgpview.io/search?query_term=cloudflare' | jq -r '.data.asns[].asn'
	  ```
   		> jq : JSON processor (-r = Raw output)<br>
     		file:  [asns.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/passive/asns.txt)

	- **Metabigor**<br>
		> command:
		```bash
		echo "cloudflare" | metabigor net --org -v | awk '{print $3}'
		```
  		> net: Discover Network Information about targets 
		Metabigor: https://github.com/j3ssie/metabigor/releases<br>
    		File:  [asnsByMetabigor.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/passive/asnsByMetabigor.txt)

* Seed Domain enumeration:
   >Enumerating seed domain helps gathering a comprehensive list of domain names associated with a target organization or entity.

	- **Amass**<br>
		command:
		```bash
		for asn in $(cat asn.txt); do                           
		amass intel -asn $asn                       
		done
	 	```
  		> File: [seed_domain.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/passive/seed_domain.txt)

* Reverse WHOIS:
  	>Reverse WHOIS is a technique used in domain and cybersecurity reconnaissance where you search for domains or IP addresses associated with a particular entity or individual using WHOIS data.

	- **Whoxy**<br>
		command:
		```bash
		curl "http://api.whoxy.com?key=<apikey>&reverse=whois&name=<domain>"
		```
  		> File: [](https://github.com/Pankaj0038/Cloudflare/blob/main/passive/whois_data.txt)
	- **DOMlink**
		>Description: It uses whoxy api to automate reverse whois

* Analytics Relationships:
  	>Analytics relationships in the context of reconnaissance (recon) refers to the process of analyzing and identifying the connections, dependencies, and patterns between various data points that provide insights into the target organization, its operations, and its relationships with other entities. 

	- **Builtwith** API with getrelation.py by **@M4ll0k**
		>code: getrelationship.py
		usage:
	  	```bash
	  	python3 getrelationship.py domain <apikey>
	  	```
 	 >File:  [related_domain.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/passive/related_domain.txt)

---

### Active scan
---
* Google Dorking:
  	>Google Dorking (also known as Google hacking) is a technique that leverages advanced search operators in Google Search to find specific information that is not easily accessible through regular search queries.

		using copyright text: "© 2024 Cloudflare, Inc."
  	> File: [dork.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/active/dork.txt)

*  Crawler:
  	>Crawler are used to systematically explore and gather information from websites and other online resources.
   	- Shodan CLI:
   	  >Command:
		```bash
		shodan search "hostname:<domain>"
   		```
	> File: [web_crawl.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/active/web_crawl.txt)
* Subdomain Enumeration:
  	>Subdomain enumeration helps to identify subdomains—smaller, often hidden sections of a website or domain—that can reveal valuable information about a target, its infrastructure, and potential attack vectors.
   	- Linked and JS discovery:
   	  	- **Burp Suite**
			>file: [burp.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/active/burp.txt)
		- **Hakrawler**
			>command:
			```bash
			echo "https://www.cloudflare.com" | hakrawler | sed 's|\(https\?://[^/]*\).*|\1|' | sort| uniq | grep -i "cloudflare"
   			```
   			> sed: to remove paths from url<br>
   	  		> sort: to sort the domain names<br>
   	  		> uniq: to remove the duplicate<br>
   	  		> grep: to fetch only the intended domain  
			> file: [subd.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/active/subd.txt)

	- Subdomain scraping:
		- **Google dork**
			>methods: site:cloudflare.com -www.cloudflare.com<br>
   			>File: [subd_dork.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/active/subd_dork.txt)

		- **Subfinder**
			>Command:
   			```bash
   			subfinder -d cloudflare.com
      		```
			>file: [subf.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/active/subf.txt)

		- **Shosubgo**
			>file: [shosub.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/active/shosub.txt)

* Service Scanning:
	
	- **Masscan**:
		>dnmasscan to convert domain name to ip and then scan for all port
		>command:
  		```bash
    	dnmasscan dom.txt dns.log -p80,443 -oG masscan.log
    	```
    	> dom.txt: domain name<br>
     	> -p80,443: providing ports to scan (eg. -p1-65535) <br>
      	> File: [masscan.log](https://github.com/Pankaj0038/Cloudflare/blob/main/active/masscan.log)

	- **nmap**:
		command:
		```bash
  		nmap -sV 104.16.133.229 -oG nmap_op.txt
  		```
  		>File: [nmap_op.txt](https://github.com/Pankaj0038/Cloudflare/blob/main/active/nmap_op.txt)
* Screenshotting:
    	> Screenshotting domains helps to save time from rendering each
  	- **Eyewitness**
  		```bash
    		eyewitness --web -f filename
    	```
    	> --web: HTTP Screenshot using Selenium<br>
     	> Folder: [2024-12-14_150314](https://github.com/Pankaj0038/Cloudflare/blob/main/active/2024-12-14_150314)

