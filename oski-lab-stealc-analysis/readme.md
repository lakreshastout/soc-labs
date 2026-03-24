# Oski Lab — Malware Analysis Report (Stealc Information Stealer)

## Lab Overview
This analysis was conducted using an ANY.RUN sandbox report and VirusTotal threat intelligence data. The objective was to examine a malicious PPT file delivered through phishing and identify command-and-control behavior, credential theft activity, encryption methods, and anti-forensics techniques.

The sample exhibited behavior consistent with an information-stealing malware family associated with Stealc/RedLine variants.

---

## Lab Source
This lab was completed as the **Oski Lab** malware analysis exercise using **ANY.RUN** and **VirusTotal**.

---

## Scenario
An accountant received an email titled **"Urgent New Order"** containing a malicious PowerPoint attachment. Upon opening the file, a SIEM alert triggered indicating a suspicious download. The file was analyzed to determine the malware's behavior, infrastructure, and post-infection actions.

---

## Analysis Summary
The malware executes a multi-stage infection chain designed to harvest credentials from browser databases, communicate with a remote command-and-control server, and remove traces of execution. The sample downloads a SQLite library to access browser credential storage, decrypts configuration strings using RC4 encryption, exfiltrates collected data, and then performs cleanup by deleting DLL files before self-deleting.

---

## Key Findings

### Malware Creation Time
The compile timestamp of the malware indicates when the sample was built.

**Creation Time**
```text
2022-09-28 17:40
```

This timestamp helps determine campaign timelines and malware reuse.

---

### Command and Control Server
The malware communicates with a remote server after execution to transmit stolen data.

**C2 Server**
```text
http://171.22.28.221/5c06c05b7b34e8e6.php
```

This HTTP communication is used for credential exfiltration.

---

### Initial Library Requested
Shortly after execution, the malware downloads an external SQLite library.

**Library**
```text
sqlite3.dll
```

This library allows the malware to access browser SQLite databases containing saved passwords, cookies, autofill data, and session tokens. This supports credential harvesting behavior.

---

### RC4 Decryption Key
The malware uses RC4 encryption to decode base64-encoded configuration strings.

**RC4 Key**
```text
5329514621441247975720749009
```

This key is used to decrypt command-and-control configuration values.

---

### Credential Theft Technique
The sandbox identified the primary MITRE ATT&CK technique used for password theft.

**MITRE Technique**
```text
T1555
```

**Technique Name:** Credential from Password Stores

This aligns with browser credential database extraction.

---

### Anti-Forensics DLL Deletion
The malware executes a cleanup command to remove DLL files after execution.

**Target Directory**
```text
C:\ProgramData
```

The deletion of DLL files indicates an attempt to remove evidence of execution.

---

### Self-Deletion Delay
After data exfiltration, the malware waits before deleting itself.

**Delay**
```text
5 seconds
```

This behavior is implemented using a timeout command before self-removal.

---

## Behavioral Analysis

The malware performs the following actions in sequence:

1. Executes the malicious payload from a phishing attachment  
2. Downloads sqlite3.dll  
3. Accesses browser credential databases  
4. Decrypts configuration using RC4  
5. Communicates with a remote C2 server  
6. Exfiltrates stolen credentials and other data  
7. Launches a child `cmd.exe` process  
8. Deletes DLL files from `C:\ProgramData`  
9. Waits 5 seconds using a timeout command  
10. Self-deletes to evade detection  

---

## Questions and Answers

### Q1
**Question:** Determining the creation time of the malware can provide insights into its origin. What was the time of malware creation?

**Answer:**
```text
2022-09-28 17:40
```

### Q2
**Question:** Identifying the command and control (C2) server that the malware communicates with can help trace back to the attacker. Which C2 server does the malware in the PPT file communicate with?

**Answer:**
```text
http://171.22.28.221/5c06c05b7b34e8e6.php
```

### Q3
**Question:** Identifying the initial actions of the malware post-infection can provide insights into its primary objectives. What is the first library that the malware requests post-infection?

**Answer:**
```text
sqlite3.dll
```

### Q4
**Question:** By examining the provided Any.run report, what RC4 key is used by the malware to decrypt its base64-encoded string?

**Answer:**
```text
5329514621441247975720749009
```

### Q5
**Question:** By examining the MITRE ATT&CK techniques displayed in the Any.run sandbox report, identify the main MITRE technique (not sub-techniques) the malware uses to steal the user’s password.

**Answer:**
```text
T1555
```

### Q6
**Question:** By examining the child processes displayed in the Any.run sandbox report, which directory does the malware target for the deletion of all DLL files?

**Answer:**
```text
C:\ProgramData
```

### Q7
**Question:** Understanding the malware's behavior post-data exfiltration can give insights into its evasion techniques. By analyzing the child processes, after successfully exfiltrating the user's data, how many seconds does it take for the malware to self-delete?

**Answer:**
```text
5
```

---

## Indicators of Interest

**C2 Server**
```text
http://171.22.28.221/5c06c05b7b34e8e6.php
```

**Dropped / Requested Library**
```text
sqlite3.dll
```

**RC4 Key**
```text
5329514621441247975720749009
```

**DLL Cleanup Directory**
```text
C:\ProgramData
```

---

## Conclusion
The analyzed sample is an information-stealing malware designed to extract browser-stored credentials and transmit them to attacker-controlled infrastructure. The use of `sqlite3.dll` indicates credential database access, while RC4 encryption is used to obfuscate configuration strings. After exfiltration, the malware performs anti-forensics cleanup and self-deletion to reduce the chance of detection.

This behavior is consistent with Stealc/RedLine-style credential-stealing malware.