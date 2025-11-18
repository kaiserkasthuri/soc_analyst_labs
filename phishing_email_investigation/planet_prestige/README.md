# BTLO - The Planet Prestige Challenge

## Overview

- This write-up documents my investigation of a suspicious email. During the analysis, I identified several indicators that suggested malicious intent. The email contained encoded text using Base64, and the attached file—while claiming to be a PDF—did not match typical PDF characteristics. After decoding the text with CyberChef, I discovered that the supposed PDF was actually a ZIP archive containing three files with unknown or misleading extensions. The following steps outline the process I used during my investigation and the findings I uncovered.
  
## Tools Used

- NotePad++
- CyberShef
- HXD
- Exftool
- PowerShell
- 7 Zip
  
## Email Analysis

1. From: "Bill" <billjobs@microapple.com>
- This email is not legitimate. It is a spoofed message that uses the name “MicroApple,” and the attacker is impersonating well-known companies to make the email appear authentic.
2. spf=fail (google.com: domain of billjobs@microapple.com does not designate 93.99.104.210 as permitted sender)
- Google flagged this message as both unauthorized and spoofed, indicating that it did not originate from the sender it claims to represent.
3. Received: from localhost (emkei.cz. [93.99.104.210])
- Emkei.cz is a well-known email spoofing service commonly used by scammers to send fraudulent or impersonated messages.
4. Reply-To: negeja3921@pashter.com
- The reply-to address points to a different email than “billjobs@microapple.com,” likely revealing the attacker’s actual address. This discrepancy is another clear indicator of suspicious activity.
  
 <img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%201.png?raw=true">
 
## Analysis Using CyberChef and File Signatures

- I used CyberChef to decode the Base64-encoded text in the email to reveal the hidden information.
- I applied the Hex operation to analyze the first four bytes (magic numbers) in order to identify the file type using known file signatures.
- Through file signature analysis, I determined that the second Base64-encoded block was actually a ZIP archive, which could be extracted on a computer.

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%202.png?raw=true">

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%203.png?raw=true">

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%205.png?raw=true">


## File Analysis

- I used the HxD tool to analyze the files. Attackers often send files without extensions to confuse or mislead the victim, making it harder to immediately identify the file type.
- I opened the Excel file using the Microsoft Excel web viewer and observed suspicious content within the sheet. After clearing the formatting, I discovered hidden Base64 text embedded in the file.

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%204.png?raw=true">

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%206.png?raw=true">


## PDF Metadata Analysis

- I used the ExifTool utility in PowerShell to examine the metadata of the file named GoodJobMayor.pdf.
- The metadata revealed additional details, including information that appears to expose the attacker’s name.

<img src="https://github.com/ImanKasthuri/soc_analyst_labs/blob/main/phishing_email_investigation/planet_prestige/screenshots/Screenshot%207.png?raw=true">

## Commands Used

- exiftool -f "GoodJobMajor.pdf"

## What I Learned

- Email spoofing is easy to perform. Attackers often use fake names, fake domains, and spoofing services such as emkei.cz to make malicious emails appear legitimate.
- An SPF failure is a strong indicator of a spoofed or unauthorized email, showing that the message did not originate from a trusted mail server.
- Base64 encoding is commonly used by attackers to conceal malicious content or hide data inside emails and attachments.
- File extensions cannot be trusted, as a file may appear to be a PDF but could actually be a ZIP archive or another file type, making it unsafe to open without verification.
- Hex editors such as HxD are extremely useful for identifying file types based on their hexadecimal signatures and magic numbers.
- Metadata can reveal valuable information, including the author, creation date, and other details that may help identify the attacker.
- CyberChef is a powerful tool for decoding, analyzing, and transforming encoded or obfuscated data.

  
  
