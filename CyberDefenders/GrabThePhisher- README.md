# GrabThePhisher Walkthrough

## Overview

This CyberDefenders lab focuses on a phishing kit made to steal MetaMask seed phrases. The files reveal the phishing code, the information collected from victims, and the methods used to store and exfiltrate it.


## Tools Used

- Kali Linux through WSL
- `find`
- `grep`
- `cat`

## Initial File Review

The lab files were opened in Kali Linux through WSL.

<img width="576" height="226" alt="Opening the GrabThePhisher folder in Kali WSL" src="https://github.com/user-attachments/assets/d27bdfac-940f-4bb9-9a10-2d0ecc0f45a6" />

The extracted directory shows the structure of the phishing kit.

<img width="731" height="503" alt="Extracted GrabThePhisher lab files" src="https://github.com/user-attachments/assets/14b5fc36-330f-4baa-9bcb-70192a9207ef" />

The `find . -type f` output lists the files available for analysis.

<img width="478" height="128" alt="Listing files in the phishing kit" src="https://github.com/user-attachments/assets/8897dddf-98e6-4d7e-a881-67eecb177a20" />

The `grep` results highlight useful strings related to wallet names, logging, and Telegram.

<img width="1898" height="781" alt="Searching the phishing kit with grep" src="https://github.com/user-attachments/assets/1acbe73a-1028-42f7-98e3-3cb1ccce87e3" />

## Questions and Findings

### Q1: Which wallet is used to ask for the seed phrase?

I used the `find` and `grep` commands to search the kit for wallet names and seed-phrase fields. The results showed a MetaMask directory and a recovery form asking the victim for an account secret phrase. Since that phrase is used to restore a MetaMask wallet, the targeted wallet was MetaMask.

**Answer:** `MetaMask`

<img width="679" height="68" alt="MetaMask wallet files found in the phishing kit" src="https://github.com/user-attachments/assets/81c7ad4f-6039-4ff8-b78b-3003f7b0aac0" />

<img width="648" height="130" alt="MetaMask seed phrase field found in the kit" src="https://github.com/user-attachments/assets/1aec1526-c94b-4cf5-a576-7158c205e142" />

### Q2: Which file contains the phishing kit code?

I used `find . -type f` to list every file in the extracted kit, then used `grep` to locate the code that processed the submitted recovery phrase. The results led to `metamask.php`, which receives the form data and contains the logging and Telegram exfiltration logic. This showed that `metamask.php` was the main phishing file.

**Answer:** `metamask.php`

<img width="1597" height="916" alt="Phishing code inside metamask.php" src="https://github.com/user-attachments/assets/be1ddf4b-d88e-46bf-a7d4-0fe51923df3a" />

### Q3: Which language was the kit written in?

I used the `cat` command to review `metamask.php` and check its file extension and server-side syntax. The code uses PHP variables and functions to process form input and send the captured information. This confirmed that the phishing kit was written in PHP.

**Answer:** `PHP`

<img width="862" height="420" alt="PHP code found in the phishing kit" src="https://github.com/user-attachments/assets/9ced23f2-ff1c-4509-8b86-3cf909c5d0f3" />

### Q4: Which service retrieves the victim's machine information?

I used `grep` to search the PHP code for URLs and functions related to IP addresses and geolocation. The result showed a request to a Sypex Geo endpoint, followed by code that read the victim's country, region, and city from the response. This identified Sypex Geo as the service used to collect the victim's machine and location information.

**Answer:** `Sypex Geo`

<img width="1271" height="73" alt="Sypex Geo lookup found in the script" src="https://github.com/user-attachments/assets/c9d73d85-93b9-40c4-bc1d-92f914d000a2" />

### Q5: How many seed phrases had already been collected?

I used `cat log.txt` to display the information already stored by the phishing kit. The output contained three separate seed-phrase records. After counting those records, the answer was three collected seed phrases.

**Answer:** `3`

<img width="825" height="97" alt="Three seed phrase entries in log.txt" src="https://github.com/user-attachments/assets/064b8d20-070d-4448-911f-86842cf0a142" />

### Q6: What was the most recent seed phrase?

I used `cat log.txt` to review the captured phrases in the order they were stored. The script appends new records to the bottom of the file, so the last entry represents the most recent phishing incident. That final entry provided the answer, but the written value is shortened because it contains wallet recovery information.

**Answer:** `father also ... hockey` *(redacted in the written answer)*

<img width="822" height="104" alt="Most recent seed phrase in log.txt" src="https://github.com/user-attachments/assets/c39399f2-efed-4f55-93fb-ef6918598385" />

### Q7: Which methods were used for credential dumping?

I used `grep` to search the PHP file for `log.txt` and Telegram-related strings. One part of the code writes the stolen information to the local log file, while another sends it through the Telegram API. This showed that the kit used local logging and Telegram as its two credential-dumping methods.

**Answer:** Local logging in `log.txt` and exfiltration through `Telegram`

<img width="1291" height="92" alt="Local logging and Telegram exfiltration code" src="https://github.com/user-attachments/assets/70996d92-60a2-4d07-acf2-0bc1ae4eae53" />

### Q8: What is the Telegram bot token?

I used `grep` to locate the Telegram API request inside the PHP script. The request contained a hardcoded value in the bot URL before the Telegram method name, showing that it was the bot token used for authentication. The written token is partially masked because it has the format of an active credential.

**Answer:** `5457463144:...xm10` *(redacted in the written answer)*

<img width="1291" height="136" alt="Telegram bot token in the phishing script" src="https://github.com/user-attachments/assets/67018ccb-fc5f-4560-80af-d9bf99359640" />

### Q9: What is the phisher's Telegram chat ID?

I used `grep` to search the Telegram request for the `chat_id` parameter. The value assigned to that parameter tells the bot where to deliver the stolen information. The number found beside `chat_id` was the phisher's Telegram chat ID.

**Answer:** `5442785564`

<img width="1216" height="266" alt="Telegram chat ID in the phishing script" src="https://github.com/user-attachments/assets/bf31979c-7e6a-4f43-aebd-152a6d95b8b4" />

### Q10: What is the phishing kit developer's alias?

I used `grep` to search the source code for comments, signatures, and unusual usernames that were not part of the normal PHP functions. The result showed the handle `j1j1b1s@m3r0` left inside the kit as a developer signature. This identified the alias connected to the phishing kit developer.

**Answer:** `j1j1b1s@m3r0`

<img width="129" height="55" alt="Developer alias found in the phishing kit" src="https://github.com/user-attachments/assets/a7a66a3e-7f41-43fd-878a-08c9b7ac9faa" />

## Key Findings

- The kit targeted MetaMask users and attempted to steal wallet recovery phrases.
- Victim location data was collected through Sypex Geo.
- Stolen data was stored in `log.txt` and sent to Telegram.

## Lessons Learned

- Simple commands such as `find` and `grep` can quickly uncover important evidence in a phishing kit.
- Hardcoded API details can reveal how stolen data is exfiltrated and who may be connected to the campaign.

Built and Documented by Aluseni Waritay
