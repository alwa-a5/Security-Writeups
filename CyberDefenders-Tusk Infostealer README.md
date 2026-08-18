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

**Thinking process:** The supplied hash matches a malicious 32-bit Windows DLL in Kaspersky's threat intelligence portal. Its file details show the size.

**Answer:** `921.36 KB`

<img width="1895" height="738" alt="Malicious file details in Kaspersky Threat Intelligence Portal" src="https://github.com/user-attachments/assets/f86d9b51-3b60-4c37-a296-796caa342ab1" />

### Q2: What word did the threat actor use for victims?

**Thinking process:** The campaign report explains that the operators used a term from Russian cybercriminal slang that compares victims to animals hunted for their tusks.

**Answer:** `Mammoth`

<img width="696" height="153" alt="Mammoth term found in campaign research" src="https://github.com/user-attachments/assets/63c29ed5-a709-4915-bd25-c750f62ab564" />

### Q3: What malicious website copied peerme.io?

**Thinking process:** The campaign report compares the legitimate PeerMe site with a look-alike domain used to deliver the malicious download.

**Answer:** `tidyme.io`

<img width="275" height="20" alt="Malicious tidyme.io domain" src="https://github.com/user-attachments/assets/ae3a2206-08dd-4e06-8c8f-cbe5290e14c0" />

### Q4: Which cloud storage service hosted the malware?

**Thinking process:** The download links for both Windows and macOS samples point to the same cloud storage provider.

**Answer:** `Dropbox`

<img width="723" height="122" alt="Dropbox malware hosting evidence" src="https://github.com/user-attachments/assets/0053f7bd-5691-4876-ae35-f8c45368ba26" />

### Q5: What was the archive decompression password?

**Thinking process:** The `config.json` file contains a hardcoded value used when extracting the downloaded archive.

**Answer:** `newfile2024`

<img width="716" height="119" alt="Archive password in config.json" src="https://github.com/user-attachments/assets/e7db9c02-1912-4483-a346-7fd1c973b765" />

### Q6: Which function retrieves the archive field?

**Thinking process:** The function that reads the archive field also decodes the URL, downloads the file, and extracts its contents.

**Answer:** `downloadAndExtractArchive`

<img width="713" height="190" alt="downloadAndExtractArchive function in preload.js" src="https://github.com/user-attachments/assets/b552fc53-52e1-49fa-b51f-91078e7152fe" />

### Q7: What were the legitimate and malicious AI translators?

**Thinking process:** The report identifies the real AI translator and the copied domain used to distribute the malicious application.

**Answer:** Legitimate: `YOUS.AI` | Malicious: `voico.io`

<img width="792" height="74" alt="YOUS.AI and voico.io translator domains" src="https://github.com/user-attachments/assets/0fe405ed-f4f0-4d3e-ab6a-494a7f7e39a5" />

### Q8: What were the StealC C2 server IP addresses?

**Thinking process:** The StealC network indicators list two IP addresses used for communication with infected systems.

**Answer:** `46.8.238.240` and `23.94.225.177`

<img width="733" height="196" alt="StealC command-and-control IP addresses" src="https://github.com/user-attachments/assets/88d3374b-7bc8-439a-b45a-03ff477bc807" />

### Q9: What Ethereum wallet was used in the campaign?

**Thinking process:** The clipper configuration contains the replacement Ethereum address used when changing clipboard contents.

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
