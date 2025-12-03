# BTLO Bruteforce

## Overview
This lab shows the RDP (Remote Desktop Protocol) brute-force attack against a Windows machine. Brute force attacks are when an attacker tries thousands of username/password combinations to break in. I used this analysis to find how many failed logins, source IP, origin country of the IP address, username, etc.

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

This command is used to filtor, how many failed audit are there.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bb3ab58f-c024-4541-a7ab-1fd92e513637" />

## What is the username of the local account that is being targeted?

The local account that is being targeted found by the CSV file (BTLO_Bruteforce_Challenge.csv)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/03f547cf-92f5-48cd-ab15-b89176b3f90e" />

## What is the failure reason related to the Audit Failure logs?

Found the answers through the same CSV  file.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0b0436ae-f82e-4c0b-b54b-78de9d6a488f" />

## What is the Windows Event ID associated with these logon failures?
I used Windows Event Viewer to check the Windows Event ID associated with logon failure

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dae5513b-9221-4bf4-9c05-f9a7ebb44095" />

## What is the source IP conducting this attack?
Everything included in the CSV file.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5eaf0d76-4d38-4ac4-9262-b8e1e00e70a6" />

## What country is this IP address associated with?
I used IP Location.Net website to check origin country.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f393e4e0-5204-4143-8385-45533e522b4a" />

And also used PowerShell command to find the origin country

```Invoke-RestMethod "https://ipinfo.io/113.161.192.227//json"```

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a6088f34-9763-44f2-bcee-af0aa40423e1" />

## What is the range of source ports that were used by the attacker to make these login requests?
First I used Notepad ++ to find the range of the source port but it's not accurate, then I used the PowerShell to gather the range of source ports

### Command I used

```powershell
# Extract ports
$ports = Select-String -Path "BTLO_Bruteforce_Challenge.txt" -Pattern "Source Port" |
ForEach-Object {
    ($_ -split "Source Port:\s*")[1].Trim() -as [int]
}

$ports | Measure-Object -Minimum -Maximum
```

### Why we check the range?

- Usually, if a normal user tries to log in, the port doesn't change; it stays the same, but if a bot tries multiple times, the port changes.
- So if all the ports all closed together as like this lab 49162-65534, it's coming from a single attacker and the same machine.
- It also helps to identify which operating system the attacker comes from. As I identified 49162-65534, these source ports are coming from a Windows Machine.
















