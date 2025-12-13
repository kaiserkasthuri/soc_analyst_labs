# RDP Bruteforce Analysis
> This investigation was completed as part of a hands-on lab from Blue Team Labs Online (BTLO).

## Senario

<img width="1740" height="361" alt="image" src="https://github.com/user-attachments/assets/d0ecc8cb-db00-45b8-97e6-22c63dc861d6" />


## Overview
This lab demonstrates an RDP (Remote Desktop Protocol) brute-force attack against a Windows machine. A brute-force attack occurs when an attacker attempts thousands of username and password combinations to gain access. In this analysis, I identified the number of failed login attempts, the source IP address, the country of origin, the username used, and other relevant details.

## Tools Used
- PowerShell
- Windows Event Viewer
- Notepad
- Notepad ++
- IP Address Lookup

## How many Audit Failure events are there?
I used PowerShell to gather Audit Failure events using this commands.

 ```$log = Import-Csv "BTLO_Bruteforce_Challenge.csv"```

The above command is used to load CSV file in to PowerShell.

```$log | Where-Object { $_.keywords -eq "Audit Failure" } | Measure-Object```

This command is used to filter how many audit failures there are.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bb3ab58f-c024-4541-a7ab-1fd92e513637" />

## What is the username of the local account that is being targeted?

The local account that was being targeted was identified using the CSV file

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/03f547cf-92f5-48cd-ab15-b89176b3f90e" />

## What is the failure reason related to the Audit Failure logs?

I found the answers through the same CSV file.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0b0436ae-f82e-4c0b-b54b-78de9d6a488f" />

## What is the Windows Event ID associated with these logon failures?
I used Windows Event Viewer to check the Windows Event ID associated with logon failures.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dae5513b-9221-4bf4-9c05-f9a7ebb44095" />

## What is the source IP conducting this attack?
Everything was included in the CSV file.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5eaf0d76-4d38-4ac4-9262-b8e1e00e70a6" />

## What country is this IP address associated with?
I used the IP-Location.net website to check the origin country..

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f393e4e0-5204-4143-8385-45533e522b4a" />

I also used a PowerShell command to find the origin country.

```Invoke-RestMethod "https://ipinfo.io/113.161.192.227//json"```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a6088f34-9763-44f2-bcee-af0aa40423e1" />

## What is the range of source ports that were used by the attacker to make these login requests?
First, I used Notepad++ to find the range of source ports, but it wasn’t accurate. I then used PowerShell to gather the correct range of source ports.

### Command I used

```powershell
# Extract ports
$ports = Select-String -Path "BTLO_Bruteforce_Challenge.txt" -Pattern "Source Port" |
ForEach-Object {
    ($_ -split "Source Port:\s*")[1].Trim() -as [int]
}

$ports | Measure-Object -Minimum -Maximum
```

### Why do we check the range?

- Usually, if a normal user tries to log in, the port doesn’t change—it stays the same. However, if a bot is attempting multiple logins, the port changes with each attempt.
-So if all the ports fall within a close range, like in this lab (49162–65534), it indicates that the traffic is coming from a single attacker using the same machine.
-It also helps identify which operating system the attacker is using. In this case, the source port range 49162–65534 indicates that the attacker is coming from a Windows machine.

## Conclusion

Working on this lab helped me get more comfortable reading and analyzing Windows logs instead of just looking at them. Using PowerShell to filter events made it easier to spot patterns and understand what was actually happening during the attack. I also learned how small details, like changing source ports, can reveal whether an attack is automated or coming from a single system. Overall, this lab helped me think more like a SOC analyst and better understand how real-world brute-force activity is investigated.
















