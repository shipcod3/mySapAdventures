# mySapAdventures
A quick methodology on testing/hacking SAP Applications for n00bz

by: Jay Turla @shipcod3

## Discovery
- Check the Application Scope for testing
- Use OSINT (open source intelligence) and Google Dorks to check for files, subdomains, and juicy information if the application is Internet-facing or public.
- Use nmap to check for open ports and known services (webdnypro, web services, web servers, etc.)
- Crawl the URLs if there is a web server running.
- Fuzz the directories (you can use Burp Intruder) if it has web servers on certain ports. Here are some good wordlists provided by the [SecLists Project](https://github.com/danielmiessler/SecLists) for finding default SAP ICM Paths and other interesting directories or files:
```
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web_Content/URLs/urls_SAP.txt
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web_Content/CMS/SAP.fuzz.txt
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web_Content/sap.txt
```
- Run [bizploit](https://www.onapsis.com/research/free-solutions)'s discovery plugins (note: customized script from davehardy20):
```
targets
	addTarget
		set host <host / ip addresses>
	back
	discoverConnectors all
	set mode sap
	back
	show
back
plugins
	discovery getClients
	discovery config getClients
	back
	discovery findRegRFCServers
	discovery config findRegRFCServers
		set regTpnames rfcexec,execute,exec,run,IGS,sapgw,sapgw00,sapgw01,sapgw02,sapgw03,NSP,GATEWAY,GATEWAY 0
	back
	discovery getSaprouterInfo
	back
	discovery getApplicationServers
	discovery icmURLScan
	discovery ping
  back
back
start
```
- Use the [SAP SERVICE DISCOVERY](https://www.rapid7.com/db/modules/auxiliary/scanner/sap/sap_service_discovery) auxiliary Metasploit module for enumerating SAP instances/services/components:
```
msf > use auxiliary/scanner/sap/sap_service_discovery
msf > set rhost <host>
msf > run
```
- Document the findings.

## Testing the Thick Client / SAP GUI
- Here is the command to connect to SAP GUI
```
sapgui <sap server hostname> <system number>
```
- Check for default credentials:
```
SAP* : 06071992, PASS
DDIC : 19920706	
TMSADM : PASSWORD, $1Pawd2&    	
SAPCPIC : ADMIN	
EARLYWATCH: SUPPORT
```
- Run Wireshark then authenticate to the client (SAP GUI) using the credentials you got because some clients transmit credentials without SSL. There are two known plugins for Wireshark that can dissect the main headers used by the SAP DIAG protocol too: [CoreLabs SAP dissection plug-in](https://www.coresecurity.com/corelabs-research/open-source-tools/sap-dissection-plug-in-wireshark) and [SAP DIAG plugin by Positive Research Center](http://blog.ptsecurity.com/2011/10/sap-diag-decompress-plugin-for.html).
- Check for privilege escalations like using some SAP Transaction Codes for low-privilege users:
```
SU01 - To create and maintain the users
SU01D - To Display Users
SU10 - For mass maintenance
SU02 - For Manual creation of profiles
SM19 - Security audit - configuration
SE84 - Information System for SAP R/3 Authorizations
```

## Testing the web interface
- Crawl the URLs (see discovery phase).
- Fuzz the URLs like in the discovery phase.
- Look for common web vulnerabilities (Refer to OWASP Top 10) because there are XSS, RCE, XXE, etc. vulnerabilities in some places.
- Check out Jason Haddix's [The Bug Hunters Methodology](https://github.com/jhaddix/tbhm) for testing web vulnerabilities.

## Attack!
- To be updated...

## References
- [SAP Penetration Testing Using Metasploit](http://information.rapid7.com/rs/rapid7/images/SAP%20Penetration%20Testing%20Using%20Metasploit%20Final.pdf)
- https://github.com/davehardy20/SAP-Stuff - a script to semi-automate Bizploit
- [SAP NetWeaver ABAP security configuration part 3: Default passwords for access to the application](https://erpscan.com/press-center/blog/sap-netweaver-abap-security-configuration-part-2-default-passwords-for-access-to-the-application/)
- [List of ABAP-transaction codes related to SAP security](https://wiki.scn.sap.com/wiki/display/Security/List+of+ABAP-transaction+codes+related+to+SAP+security)
