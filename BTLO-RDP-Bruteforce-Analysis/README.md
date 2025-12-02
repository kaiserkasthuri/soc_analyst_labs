# BTLO Bruteforce

## Overview

## Tools Used

## Question 1) How many Audit Failure events are there?
I used PowerShell to gather Audit Failure events using this commands.

1. $log = Import-Csv "BTLO_Bruteforce_Challenge.csv"

The above command is used to load CSV file in to PowerShell.

2. $log | Where-Object { $_.keywords -eq "Audit Failure" } | Measure-Object

This command is used to filtor, how many failed audit are there.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bb3ab58f-c024-4541-a7ab-1fd92e513637" />

## What is the username of the local account that is being targeted?

The local account that is being targeted found by the CSV file (BTLO_Bruteforce_Challenge.csv)

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/03f547cf-92f5-48cd-ab15-b89176b3f90e" />

## What is the failure reason related to the Audit Failure logs?

Found the answers through the same CSV  file.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0b0436ae-f82e-4c0b-b54b-78de9d6a488f" />

##  What is the Windows Event ID associated with these logon failures?
I used Windows Event Viewer to check the Windows Event ID associated with logon failure

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/dae5513b-9221-4bf4-9c05-f9a7ebb44095" />

## What is the source IP conducting this attack?
Everything included in the CSV file.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5eaf0d76-4d38-4ac4-9262-b8e1e00e70a6" />

## What country is this IP address associated with?
I used IP Location.Net website to check origin country.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f393e4e0-5204-4143-8385-45533e522b4a" />

And also used PowerShell command to find the origin country

Invoke-RestMethod "https://ipinfo.io/113.161.192.227//json"

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a6088f34-9763-44f2-bcee-af0aa40423e1" />















