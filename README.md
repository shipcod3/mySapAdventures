# mySapAdventures
A quick methodology on testing SAP Applications for n00bz
by: Jay Turla @shipcod3

## Discovery
- Check the Application Scope for testing
- Use OSINT (open source intelligence) and Google Dorks to check for files, subdomains, and juicy information if the application is Internet-facing or public.
- Use nmap to check for open ports and known services (webdnypro, web services, web servers, etc.)
- Fuzz the directories (you can use Burp Intruder) if it has web servers on certain ports. Here are some good wordlists provided by the [SecLists Project](https://github.com/danielmiessler/SecLists) for looking default SAP ICM Paths and other interesting directories or files:
```
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web_Content/URLs/urls_SAP.txt \s\s
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web_Content/CMS/SAP.fuzz.txt
https://github.com/danielmiessler/SecLists/blob/master/Discovery/Web_Content/sap.txt
```
- 


```
sapgui <sap server hostname> <system number>
```
