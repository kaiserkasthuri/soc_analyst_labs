# BTLO - The Planet Prestige Challenge

## Overview

- Today I took a challenge on BTLO - Planet Prestige, which is about investigating a malicious email. During the analysis, I found that the email contained a file that claimed to be a PDF file, but it did not meet the characteristics of a PDF file. I discovered that some of the texts were encoded with base64. I decoded the text with the help of CyberShef and found that the content was actually a ZIP file. I extracted the ZIP file on my computer and discovered that three of the files did not have a file extension. This report shows how I did my investigation from different tools and techniques.
  
## Tools Used

- NotePad++
- CyberShef
- HXD
- Exftool
- PowerShell
- 7 Zip
  
## Email Analysis

1. From: "Bill" <billjobs@microapple.com>
- This sender's address is not legitimate; the attacker used the well-known companies' domains to appear credible.
2. spf=fail (google.com: domain of billjobs@microapple.com does not designate 93.99.104.210 as permitted sender)
- This indicates that the email came from a different server.
3. Received: from localhost (emkei.cz. [93.99.104.210])
-  emkei.cz is a fake email-generating website; SPF fails because the email came from emkei.cz instead of the real server.
4. Reply-To: negeja3921@pashter.com
- The reply is going to a different address instead of the from address, which is suspicious, and this is the real email address that the attacker uses to send mail.
  
 <img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%201.png?raw=true">
 
## Analysis Using CyberChef and File Signatures

- I used CyberShef to decode base64 text and discover the hidden information.
- I used the CyberShef Hex operator to decode the content and found the first four bytes (Magic Numbers). 
- With the help of magic numbers, I used file signatures to determine what kind of file it is. The attacker claimed that it's a PDF file, but the File Signatures website has helped me to find out that it’s a ZIP archive file.
  
<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%202.png?raw=true">

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%203.png?raw=true">

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%205.png?raw=true">


## File Analysis

- I used HxD to examine the extracted files. Attackers usually remove the file extension to confuse the victim. So the victim unknowingly opens the file and executes malware on the system.
- One of the files opened in Excel showed suspicious, unreadable content. After removing the cell formatting, I found that hidden Base64 text is in sheet 3.

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%204.png?raw=true">

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%206.png?raw=true">


## PDF Metadata Analysis

- I used exiftool in PowerShell to view the metadata of the GoodJobMajor.pdf file.
- The metadata revealed so much information, including the author's name, file access date, creation date, and the attacker's real name.
<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%207.png?raw=true">

## Commands Used

- exiftool -f "GoodJobMajor.pdf"

## What I Learned

- I learned that attackers used various methods to do email spoofing, for example use of fake email generators like emkei.cz, to make fake domains for emails to convince it’s legitimacy.
- SPF failure is one of the biggest red flags that the email is spoofed or unauthorized, and should always be investigated.
- Base64 encoding is frequently used to hide malicious content and embedded data.
- File extensions cannot be trusted, because some of the files claimed to be safe, but they could be zip archives or other dangerous formats.
- HxD hex editor is essential to identify the file type using magic numbers.
- Metadata analysis can reveal useful details about file authorship, timestamp, and attackers' names or information.
- CyberShef is a powerful tool for decoding and analyzing encoded data.
  

  
  
