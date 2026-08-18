# Tusk Infostealer Walkthrough

## Overview

In this CyberDefenders lab, I investigated the Tusk infostealer campaign targeting blockchain and cryptocurrency users. I used threat intelligence sources to connect the malicious file to fake websites, second-stage malware, command-and-control servers, and an attacker-controlled Ethereum wallet.

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

**Answer:** `921.36 KB`

I searched the supplied file hash in Kaspersky's threat intelligence portal. The file details showed that it was a malicious 32-bit Windows DLL with a size of 921.36 KB.

<img width="1895" height="738" alt="Malicious file details in Kaspersky Threat Intelligence Portal" src="https://github.com/user-attachments/assets/f86d9b51-3b60-4c37-a296-796caa342ab1" />

### Q2: What word did the threat actor use for victims?

**Answer:** `Mammoth`

The campaign operators called their victims Mammoths. The term comes from Russian cybercriminal slang and compares victims to the mammoths hunted for their tusks.

<img width="696" height="153" alt="Mammoth term found in campaign research" src="https://github.com/user-attachments/assets/63c29ed5-a709-4915-bd25-c750f62ab564" />

### Q3: What malicious website copied peerme.io?

**Answer:** `tidyme.io`

The attackers created `tidyme.io` to imitate the legitimate PeerMe DAO platform and lead victims to a malicious download.

<img width="275" height="20" alt="Malicious tidyme.io domain" src="https://github.com/user-attachments/assets/ae3a2206-08dd-4e06-8c8f-cbe5290e14c0" />

### Q4: Which cloud storage service hosted the malware?

**Answer:** `Dropbox`

Dropbox was used to host malware samples for both Windows and macOS.

<img width="723" height="122" alt="Dropbox malware hosting evidence" src="https://github.com/user-attachments/assets/0053f7bd-5691-4876-ae35-f8c45368ba26" />

### Q5: What was the archive decompression password?

**Answer:** `newfile2024`

The password was hardcoded in `config.json` and used to extract the downloaded archive containing the next-stage payloads.

<img width="716" height="119" alt="Archive password in config.json" src="https://github.com/user-attachments/assets/e7db9c02-1912-4483-a346-7fd1c973b765" />

### Q6: Which function retrieves the archive field?

**Answer:** `downloadAndExtractArchive`

The function decoded the archive URL, downloaded the password-protected file, extracted it, and ran the included executables.

<img width="713" height="190" alt="downloadAndExtractArchive function in preload.js" src="https://github.com/user-attachments/assets/b552fc53-52e1-49fa-b51f-91078e7152fe" />

### Q7: What were the legitimate and malicious AI translators?

**Answer:** Legitimate: `YOUS.AI` | Malicious: `voico.io`

The attackers copied YOUS.AI and used `voico.io` to distribute a malicious downloader disguised as a translator application.

<img width="792" height="74" alt="YOUS.AI and voico.io translator domains" src="https://github.com/user-attachments/assets/0fe405ed-f4f0-4d3e-ab6a-494a7f7e39a5" />

### Q8: What were the StealC C2 server IP addresses?

**Answer:** `46.8.238.240` and `23.94.225.177`

These IP addresses were used by StealC to communicate with infected systems and receive stolen information.

<img width="733" height="196" alt="StealC command-and-control IP addresses" src="https://github.com/user-attachments/assets/88d3374b-7bc8-439a-b45a-03ff477bc807" />

### Q9: What Ethereum wallet was used in the campaign?

**Answer:** `0xaf0362e215Ff4e004F30e785e822F7E20b99723A`

The clipper malware replaced copied wallet addresses with this attacker-controlled address so victims could send funds to the attacker by mistake.

<img width="443" height="37" alt="Ethereum wallet used in the Tusk campaign" src="https://github.com/user-attachments/assets/28b42c83-1d01-472d-85e1-9da85802f88b" />

## Key Findings

- The campaign used fake blockchain and AI websites to trick victims into downloading malware.
- Dropbox hosted the initial malware, which later delivered StealC and other payloads.
- The attackers used C2 servers and clipper malware to steal information and cryptocurrency.

## Lessons Learned

- File hashes can connect a sample to a larger campaign through threat intelligence research.
- Domains, IP addresses, configuration files, and wallet addresses all help build the full attack picture.

Built and Documented by Aluseni Waritay
