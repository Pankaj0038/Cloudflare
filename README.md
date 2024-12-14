# Cloudflare
---
### Passive Recon

---

* Acquisitions:
  	>Acquisitions of a company refer to the businesses, assets, or companies that a particular company has purchased or taken control. Acquisitions provides a holistic understanding of a company’s strategies, market positioning, risks, and potential for growth.
	- **Crunchbase**
   		>Crunchbase is a comprehensive platform and database that provides information about businesses, startups, investors, and industry trends. It is widely used by entrepreneurs, investors, researchers, and analysts to gather data on companies, track funding rounds, discover new startups, and understand the overall startup ecosystem.

* ASN Enumeration:
  	>An ASN (Autonomous System Number) is a unique identifier assigned to an Autonomous System (AS) in the context of BGP (Border Gateway Protocol) routing on the internet.

	- **BGP view api** <br>
		> command:
	  ```bash
	  curl 'https://api.bgpview.io/search?query_term=cloudflare' | jq -r '.data.asns[].asn'
	  ```

	- **Metabigor**<br>
		> command:
		```bash
			echo "cloudflare" | metabigor net --org -v | awk '{print $3}'
		```
		Metabigor: https://github.com/j3ssie/metabigor/releases

* Seed Domain enumeration:
   >Enumerating seed domain helps gathering a comprehensive list of domain names associated with a target organization or entity.

	- **Amass**<br>
		command:
		```bash
		for asn in $(cat asn.txt); do                           
		amass intel -asn $asn                       
		done
	 	```

* Reverse WHOIS:
  	>Reverse WHOIS is a technique used in domain and cybersecurity reconnaissance where you search for domains or IP addresses associated with a particular entity or individual using WHOIS data.

	- **Whoxy**<br>
		command:
		```bash
		curl "http://api.whoxy.com?key=<apikey>&reverse=whois&name=<domain>"
		```

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
