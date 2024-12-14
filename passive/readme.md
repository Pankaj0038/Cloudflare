# Cloudflare

1. Acquisitions:
		Acquisitions of a company refer to the businesses, assets, or companies that a particular company has purchased or taken control. Acquisitions provides a holistic understanding of a company’s strategies, market positioning, risks, and potential for growth.

	**Crunchbase** 

2. ASN Enumeration:
		An ASN (Autonomous System Number) is a unique identifier assigned to an Autonomous System (AS) in the context of BGP (Border Gateway Protocol) routing on the internet.

	**BGP view api** <br>
		command: ```bash
				curl 'https://api.bgpview.io/search?query_term=cloudflare' | jq -r '.data.asns[].asn' ```

	**Metabigor**<br>
		command:
			```bash
				echo "cloudflare" | metabigor net --org -v | awk '{print $3}'
			```<br>
		Metabigor: https://github.com/j3ssie/metabigor/releases

4. Seed Domain enumeration:
		Enumerating seed domain helps gathering a comprehensive list of domain names associated with a target organization or entity.

	**Amass**<br>
		command: ```bash
				for asn in $(cat asn.txt); do                           
				amass intel -asn $asn                       
				done ```

5. Reverse WHOIS:
		Reverse WHOIS is a technique used in domain and cybersecurity reconnaissance where you search for domains or IP addresses associated with a particular entity or individual using WHOIS data.

	**Whoxy**<br>
		command: ```bash
			curl "http://api.whoxy.com?key=<apikey>&reverse=whois&name=<domain>"
			```

	**DOMlink**<br>
		Description: It uses whoxy api to automate reverse whois

6. Analytics Relationships:
		Analytics relationships in the context of reconnaissance (recon) refers to the process of analyzing and identifying the connections, dependencies, and patterns between various data points that provide insights into the target organization, its operations, and its relationships with other entities. 

	**Builtwith** API with getrelation.py by **@M4ll0k**<br>
			code: getrelationship.py
			usage: ```bash
				python3 getrelationship.py domain <apikey>```
