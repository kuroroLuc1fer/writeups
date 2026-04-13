# Ghostcat (CVE-2020-1938)

## Summary

A security assessment identified a critical vulnerability chain in Apache Tomcat. By leveraging leaked credentials and an unrestricted WAR file upload, an attacker can achieve Remote Code Execution (RCE). Furthermore, the server is vulnerable to CVE-2020-1938 (Ghostcat), allowing unauthorized access to sensitive configuration files.

## Vulnerability & writeups

The main vulnerability aside others was **Ghostcat** known as CVE-2020-1938 targeting Apache Tomcat 9.0.0.M1 to 9.0.0.30, 8.5.0 to 8.5.50 and 7.0.0 to 7.0.99; the web application that was tested seemed to be in those versions. The test started by a nmap scan disclosing three opened ports (22/ssh, 8009/AJP and 8080/Webapp), after what *msfvenom* was used to craft a payload allowing a reverse shell, visiting the webapp there was an authentication mechanism that exposes valid credentials after an authentication failure. The webapp, after authentication success leaded to an interface allowing to upload .war files, after uploading the payload, it was triggered by visiting the concerned URL, the attacker waiting for the connection via *netcat*, privilege escalation was made because the file that was executed by root as cron job could be edited by anyone promoting them to root privileges.

## Appendices

After those investigations there are files left on the server *shell.war* pointing to a specific IP and PORT, deleting it is recommanded and use strong credentials, avoid to divulge them in authentication failure.

* Update Tomcat: Move to versions 9.0.31, 8.5.51, or 7.0.100 or higher.

* Disable AJP: If not required, comment out the AJP Connector in server.xml.

* Hardening: Implement the Principle of Least Privilege—no script executed by a cron job should be world-writable.

* Input Validation: Sanitize file upload types to prevent WAR file deployment by unauthorized users.