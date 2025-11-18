# BTLO - The planet Prestige Challenge

## Overview
- This write-up is about documenting the email that I investigated, and I found some suspicious information. I found out that there are some encoded texts with base64, and this email has a file pretending to be a PDF file. I used CyberShef to decode the text file and found out that it's a zip file, it has three files, which I don't know the filextensions. The following steps show how I did my investigation.

## Tool Used

- NotePad++
- CyberShef
- HXD
- Exftool
- PowerShell
- 7 Zip
  
## Email Analysis

1. From: "Bill" <billjobs@microapple.com>
- This is not a legitimate email; the email is spoofed and uses MicroApple, and the attacker used famous companies' names for the email.
2. spf=fail (google.com: domain of billjobs@microapple.com does not designate 93.99.104.210 as permitted sender)
- Google detected that this is an unauthorized email and a spoofed email.
3. Received: from localhost (emkei.cz. [93.99.104.210])
- Emkei.cz is a famous email spoofing website, used by scammers to send fake emails.
4. Reply-To: negeja3921@pashter.com
- This reply is going to a different email which means attackers real address, instead of "billjobs@microapple.com". This is suspicious.

## CyberShef/File Signature

- I used CyberShef to decode the encoded text in the email to know the hidden information.
- Used operation Hex to decode the first 4 bytes magic numbers and to see which file extension by using File Signature.
- Found that the second base64 text is a zip file using File Signature, which can be extracted on a computer.

## Analysing The Files

- I used the HXD Tool to analyse the files. Attackers often send files without an extension to confuse the victim.
- Opened the Excel file using the Microsoft Excel website and noticed that some suspicious text in the Excel. I cleared the format and found that base64 text is there.

## Viewing the Metadata of PDF a File

- I used the Exftool tool to view the metadata of the GoodJobMayor.pdf file, using PowerShell.
- The metadata revealed the details such as attacker name.

## Commands Used

- exiftool -f "GoodJobMajor.pdf"

## What I Learned

- Email spoofing is easy to do. Attackers often use fake names, fake domains, and use fake email generators like emkei.cz to make emails look real.
- SPF fail is a strong indicator of a fake email, which means it did not come from a trusted server.
- Base64 is commonly used by attackers to hide information.
- File extension cannot be trusted, because the file can pretend to be a PDF file, but it could be a ZIP file, and it's not safe to open.
- Hex editors like HXD are extremely useful to identify what type of file it is.
- Metadata can reveal important information such as author, creation date, etc.
- Cybershef is a powerful tool for decoding the text.

  
  
