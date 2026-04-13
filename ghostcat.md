# Ghostcat (CVE-2020-1938)

## Summary

A web application was tested, it had credentials leaks after login failure, allowing threat actors to upload malicious files via a web interface leading to remote access to servers.

## Vulnerability & writeups

The main vulnerability aside others was **Ghostcat** known as CVE-2020-1938 targeting Apache Tomcat 9.0.0.M1 to 9.0.0.30, 8.5.0 to 8.5.50 and 7.0.0 to 7.0.99; the web application that was tested seemed to be in those versions. The test started by a nmap scan disclosing three opened ports (22/ssh, 8009/AJT and 8080/Webapp), after what *msfvenom* was used to craft a payload allowing a reverse shell, visiting the webapp there was an authentication mechanism that exposes valid credentials after an authentication failure. The webapp, after authentication success leaded to an interface allowing to upload .war files, after uploading the payload, it was triggered by visiting the concerned URL, the attacker waiting for the connection via *netcat*, privilege escalation was made because the file that was executed by root as cron job could be edited by anyone promoting them to root privileges.

## Appendices

After those investigations there are files left on the server *shell.war* pointing to a specific IP and PORT, deleting it is recommanded and use strong credentials, avoid to divulge them in authentication failure, the main solution is to upgrade the tomcat version.