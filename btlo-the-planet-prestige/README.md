# BTLO - The Planet Prestige Challenge

## Overview

- Today I completed the BTLO challenge Planet Prestige, which focuses on investigating a malicious email. During the analysis, I found that the email included a file that claimed to be a PDF, but it did not match the characteristics of a legitimate PDF file. I also discovered that some of the text within the email was encoded in Base64. After decoding it using CyberChef, I learned that the content was actually a ZIP file. When I extracted the ZIP file on my computer, I found that three of the files inside did not have a file extension. This report outlines the tools and techniques I used throughout my investigation and the steps I took to analyze the email and its attachments.
  
## Tools Used

- NotePad++
- CyberShef
- HXD
- Exftool
- PowerShell
- 7 Zip
  
## Email Analysis

1. From: "Bill" <billjobs@microapple.com>
- The sender’s address is not legitimate; the attacker used a well-known company’s domain to appear credible.
2. spf=fail (google.com: domain of billjobs@microapple.com does not designate 93.99.104.210 as permitted sender)
- This indicates that the email came from a different server.
3. Received: from localhost (emkei.cz. [93.99.104.210])
-  emkei.cz is a fake email-generating website; the SPF check fails because the email was sent from emkei.cz instead of the legitimate server.
4. Reply-To: negeja3921@pashter.com
– The reply is directed to a different address instead of the “from” address, which is suspicious. This is the real email address the attacker uses to receive replies.
  
 <img src="https://github.com/imankasthuri/soc-analyst-labs/blob/main/btlo-the-planet-prestige/screenshots/Screenshot%201.png?raw=true">
 
## Analysis Using CyberChef and File Signatures

- I used CyberChef to decode the Base64 text and uncover the hidden information.
-I used the CyberChef Hex operator to decode the content and identified the first four bytes (magic numbers).
- Using the magic numbers, I checked the file signatures to determine what type of file it actually was. Although the attacker claimed it was a PDF file, the File Signatures website helped me confirm that it was actually a ZIP archive.
  
<img src="https://github.com/imankasthuri/soc-analyst-labs/blob/main/btlo-the-planet-prestige/screenshots/Screenshot%202.png?raw=true">

<img src="https://github.com/imankasthuri/soc-analyst-labs/blob/main/btlo-the-planet-prestige/screenshots/Screenshot%203.png?raw=true">

<img src="https://github.com/imankasthuri/soc-analyst-labs/blob/main/btlo-the-planet-prestige/screenshots/Screenshot%204.png?raw=true">


## File Analysis

- I used HxD to examine the extracted files. Attackers often remove file extensions to confuse the victim, causing them to unknowingly open the file and execute malware on their system.
- One of the files opened in Excel contained suspicious, unreadable content. After removing the cell formatting, I found hidden Base64 text in Sheet 3.

<img src="https://github.com/imankasthuri/soc-analyst-labs/blob/main/btlo-the-planet-prestige/screenshots/Screenshot%205.png?raw=true">

<img src="https://github.com/imankasthuri/soc-analyst-labs/blob/main/btlo-the-planet-prestige/screenshots/Screenshot%206.png?raw=true">


## PDF Metadata Analysis

- I used exiftool in PowerShell to view the metadata of the GoodJobMajor.pdf file.
- The metadata revealed a lot of information, including the author’s name, the file access date, the creation date, and the attacker’s real name.
<img src="https://github.com/imankasthuri/soc-analyst-labs/blob/main/btlo-the-planet-prestige/screenshots/Screenshot%207.png?raw=true">

## Commands Used

- exiftool -f "GoodJobMajor.pdf"

## What I Learned

- I learned that attackers use various methods to perform email spoofing, such as using fake email generators like emkei.cz or creating fake domains to make their emails appear legitimate.
- An SPF failure is one of the biggest red flags that an email is spoofed or unauthorized, and it should always be investigated.
-Base64 encoding is frequently used to hide malicious content or embedded data.
- File extensions cannot be trusted, because some files may appear safe but could actually be ZIP archives or other dangerous formats.
- The HxD hex editor is essential for identifying file types using magic numbers.
-Metadata analysis can reveal useful details about file authorship, timestamps, and attacker-related information.
- CyberChef is a powerful tool for decoding and analyzing encoded data.
  

  
  
