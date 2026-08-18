# Tusk Infostealer Walkthrough

## Overview

This CyberDefenders lab focuses on the Tusk infostealer campaign targeting blockchain and cryptocurrency users. Threat intelligence connects the malicious file to fake websites, second-stage malware, command-and-control servers, and an attacker-controlled Ethereum wallet.

## Challenge Details

| Field | Details |
|---|---|
| Platform | CyberDefenders |
| Category | Threat Intelligence and Malware Analysis |
| Status | Completed |
| Lab | [Tusk Infostealer](https://cyberdefenders.org/blueteam-ctf-challenges/tusk-infostealer/) |

## Tools Used

- Kaspersky Threat Intelligence Portal
- Securelist threat research
- VirusTotal

## Questions and Findings

### Q1: What is the size of the malicious file?

I used the supplied MD5 hash to search for the sample in Kaspersky's Threat Intelligence Portal. The lookup matched the hash to a malicious 32-bit Windows DLL and displayed its file metadata. The size field showed that the file was 921.36 KB.

**Answer:** `921.36 KB`

<img width="1895" height="738" alt="Malicious file details in Kaspersky Threat Intelligence Portal" src="https://github.com/user-attachments/assets/f86d9b51-3b60-4c37-a296-796caa342ab1" />

### Q2: What word did the threat actor use for victims?

I used the sample's hash and campaign details to locate the related Securelist report. The report explained that the operators called their victims “Mammoths,” a term from Russian cybercriminal slang that compares victims to animals hunted for valuable tusks. This made `Mammoth` the answer.

**Answer:** `Mammoth`

<img width="696" height="153" alt="Mammoth term found in campaign research" src="https://github.com/user-attachments/assets/63c29ed5-a709-4915-bd25-c750f62ab564" />

### Q3: What malicious website copied peerme.io?

I used the campaign report to compare the real PeerMe platform with the attacker's copied website. The fake site reused the DAO theme and included a download button that delivered malware instead of the legitimate application. The domain listed for that malicious copy was `tidyme.io`.

**Answer:** `tidyme.io`

<img width="275" height="20" alt="Malicious tidyme.io domain" src="https://github.com/user-attachments/assets/ae3a2206-08dd-4e06-8c8f-cbe5290e14c0" />

### Q4: Which cloud storage service hosted the malware?

I used the documented download links to compare the Windows and macOS versions of the malware. Both links pointed to files hosted on the same cloud storage platform. The URLs showed that the campaign operators used Dropbox.

**Answer:** `Dropbox`

<img width="723" height="122" alt="Dropbox malware hosting evidence" src="https://github.com/user-attachments/assets/0053f7bd-5691-4876-ae35-f8c45368ba26" />

### Q5: What was the archive decompression password?

I used the `config.json` data to find the value passed to the archive-extraction routine. The configuration contained encoded payload URLs and a hardcoded decompression password. The password stored in that field was `newfile2024`.

**Answer:** `newfile2024`

<img width="716" height="119" alt="Archive password in config.json" src="https://github.com/user-attachments/assets/e7db9c02-1912-4483-a346-7fd1c973b765" />

### Q6: Which function retrieves the archive field?

I used `preload.js` to trace the function that referenced the `archive` field from `config.json`. The same function decoded the URL, downloaded the archive, extracted it with the configured password, and ran its executable files. Its name was `downloadAndExtractArchive`.

**Answer:** `downloadAndExtractArchive`

<img width="713" height="190" alt="downloadAndExtractArchive function in preload.js" src="https://github.com/user-attachments/assets/b552fc53-52e1-49fa-b51f-91078e7152fe" />

### Q7: What were the legitimate and malicious AI translators?

I used the third sub-campaign section of the threat report to compare the legitimate AI translator with the attacker's copy. The report named YOUS.AI as the real service and showed that `voico.io` copied it to distribute a malicious downloader. This provided both the legitimate and malicious translator names.

**Answer:** Legitimate: `YOUS.AI` | Malicious: `voico.io`

<img width="792" height="74" alt="YOUS.AI and voico.io translator domains" src="https://github.com/user-attachments/assets/0fe405ed-f4f0-4d3e-ab6a-494a7f7e39a5" />

### Q8: What were the StealC C2 server IP addresses?

I used the network-indicator section of the campaign report to find the entries labeled as StealC command-and-control infrastructure. Two IP addresses were linked to communications between StealC infections and the attacker's servers. Those addresses were `46.8.238.240` and `23.94.225.177`.

**Answer:** `46.8.238.240` and `23.94.225.177`

<img width="733" height="196" alt="StealC command-and-control IP addresses" src="https://github.com/user-attachments/assets/88d3374b-7bc8-439a-b45a-03ff477bc807" />

### Q9: What Ethereum wallet was used in the campaign?

I used the clipper configuration to find the Ethereum address stored as the clipboard replacement value. The malware monitored copied cryptocurrency addresses and replaced matching Ethereum addresses with the attacker's wallet. The configured replacement address was `0xaf0362e215Ff4e004F30e785e822F7E20b99723A`.

**Answer:** `0xaf0362e215Ff4e004F30e785e822F7E20b99723A`

<img width="443" height="37" alt="Ethereum wallet used in the Tusk campaign" src="https://github.com/user-attachments/assets/28b42c83-1d01-472d-85e1-9da85802f88b" />

## Key Findings

- The campaign used fake blockchain and AI websites to trick victims into downloading malware.
- Dropbox hosted the initial malware, which later delivered StealC and other payloads.
- The attackers used C2 servers and clipper malware to steal information and cryptocurrency.

## Lessons Learned

- File hashes can connect a sample to a larger campaign through threat intelligence research.
- Domains, IP addresses, configuration files, and wallet addresses all help build the full attack picture.

Built and Documented by Aluseni Waritay
