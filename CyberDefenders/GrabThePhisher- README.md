# GrabThePhisher Walkthrough

## Overview

This CyberDefenders lab focuses on a phishing kit made to steal MetaMask seed phrases. The files reveal the phishing code, the information collected from victims, and the methods used to store and exfiltrate it.

## Challenge Details

| Field | Details |
|---|---|
| Platform | CyberDefenders |
| Category | Phishing Analysis |
| Status | Completed |
| Lab | [GrabThePhisher](https://cyberdefenders.org/blueteam-ctf-challenges/grabthephisher/) |

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

The kit contains a directory and recovery page built around MetaMask. The form asks the victim for an account recovery phrase, which is the seed phrase used to restore a MetaMask wallet. These details identify MetaMask as the wallet being targeted.

**Answer:** `MetaMask`

<img width="679" height="68" alt="MetaMask wallet files found in the phishing kit" src="https://github.com/user-attachments/assets/81c7ad4f-6039-4ff8-b78b-3003f7b0aac0" />

<img width="648" height="130" alt="MetaMask seed phrase field found in the kit" src="https://github.com/user-attachments/assets/1aec1526-c94b-4cf5-a576-7158c205e142" />

### Q2: Which file contains the phishing kit code?

The file list shows a server-side file named `metamask.php`. Reviewing its contents shows that it receives the submitted recovery phrase, formats the stolen information, and handles the logging and exfiltration steps. This makes it the main file responsible for the phishing kit's operation.

**Answer:** `metamask.php`

<img width="1597" height="916" alt="Phishing code inside metamask.php" src="https://github.com/user-attachments/assets/be1ddf4b-d88e-46bf-a7d4-0fe51923df3a" />

### Q3: Which language was the kit written in?

The main phishing file uses the `.php` extension and contains PHP server-side syntax. Its code processes form input, works with variables, and sends information after the victim submits the page. These features confirm the language used by the kit.

**Answer:** `PHP`

<img width="862" height="420" alt="PHP code found in the phishing kit" src="https://github.com/user-attachments/assets/9ced23f2-ff1c-4509-8b86-3cf909c5d0f3" />

### Q4: Which service retrieves the victim's machine information?

The script collects the victim's IP address and sends it to a Sypex Geo endpoint. It then reads location fields such as the country, region, and city from the response. This shows that Sypex Geo is the service used to gather information about the victim's system and location.

**Answer:** `Sypex Geo`

<img width="1271" height="73" alt="Sypex Geo lookup found in the script" src="https://github.com/user-attachments/assets/c9d73d85-93b9-40c4-bc1d-92f914d000a2" />

### Q5: How many seed phrases had already been collected?

The `log.txt` file stores each captured seed phrase as a separate entry. Counting the distinct entries in the file gives a total of three. This means the phishing kit had already collected three seed phrases when the files were examined.

**Answer:** `3`

<img width="825" height="97" alt="Three seed phrase entries in log.txt" src="https://github.com/user-attachments/assets/064b8d20-070d-4448-911f-86842cf0a142" />

### Q6: What was the most recent seed phrase?

The phishing script adds newly captured information to `log.txt` instead of replacing the earlier entries. Because the records are appended in order, the entry at the bottom of the file represents the latest phishing incident. The written answer is shortened because it contains wallet recovery information.

**Answer:** `father also ... hockey` *(redacted in the written answer)*

<img width="822" height="104" alt="Most recent seed phrase in log.txt" src="https://github.com/user-attachments/assets/c39399f2-efed-4f55-93fb-ef6918598385" />

### Q7: Which methods were used for credential dumping?

The script uses two separate methods to keep the stolen information. One section writes the captured data to the local `log.txt` file, while another sends the same data through the Telegram API. Local storage provides a backup, and Telegram gives the attacker near real-time access to new submissions.

**Answer:** Local logging in `log.txt` and exfiltration through `Telegram`

<img width="1291" height="92" alt="Local logging and Telegram exfiltration code" src="https://github.com/user-attachments/assets/70996d92-60a2-4d07-acf2-0bc1ae4eae53" />

### Q8: What is the Telegram bot token?

The PHP code builds a Telegram API request using a hardcoded bot token. The token appears in the request path before the Telegram method name, which allows the bot to authenticate with the service. The written value is partially masked because it has the format of an active credential.

**Answer:** `5457463144:...xm10` *(redacted in the written answer)*

<img width="1291" height="136" alt="Telegram bot token in the phishing script" src="https://github.com/user-attachments/assets/67018ccb-fc5f-4560-80af-d9bf99359640" />

### Q9: What is the phisher's Telegram chat ID?

The Telegram request contains a `chat_id` parameter along with the stolen message. This value tells the bot which user, group, or channel should receive the information. The number assigned to that parameter is therefore the phisher's chat ID.

**Answer:** `5442785564`

<img width="1216" height="266" alt="Telegram chat ID in the phishing script" src="https://github.com/user-attachments/assets/bf31979c-7e6a-4f43-aebd-152a6d95b8b4" />

### Q10: What is the phishing kit developer's alias?

The source code contains a handle that is separate from the normal variables and phishing functions. Its placement looks like a developer signature or attribution left inside the kit. That handle identifies the alias associated with the phishing kit developer.

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
