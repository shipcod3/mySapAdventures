# mySapAdventures
A quick methodology on testing/hacking SAP Applications for n00bz

by: Jay Turla @shipcod3

## Discovery
- Check the Application Scope for testing
- Use OSINT (open source intelligence) and Google Dorks to check for files, subdomains, and juicy information if the application is Internet-facing or public.
- Use nmap to check for open ports and known services (webdnypro, web services, web servers, etc.)
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
- Use the sap_service_discovery auxiliary Metasploit module for enumerating SAP instances/services/components:
```
msf > use auxiliary/scanner/sap/sap_service_discovery
msf > set rhost <host>
msf > run
```

```
sapgui <sap server hostname> <system number>
```

## References
- [SAP Penetration Testing Using Metasploit](http://information.rapid7.com/rs/rapid7/images/SAP%20Penetration%20Testing%20Using%20Metasploit%20Final.pdf)
- https://github.com/davehardy20/SAP-Stuff - a script to semi-automate Bizploit
