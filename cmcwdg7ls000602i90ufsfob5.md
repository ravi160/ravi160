---
title: "Malware Analysis for Beginners: How to Investigate a Suspicious File"
seoTitle: "Malware Analysis for Beginners: How to Investigate a Suspicious File"
datePublished: Wed Jul 09 2025 19:48:27 GMT+0000 (Coordinated Universal Time)
cuid: cmcwdg7ls000602i90ufsfob5
slug: malware-analysis-for-beginners-how-to-investigate-a-suspicious-file
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1752089731263/a1381ae7-94f3-4376-a6cd-9432f3d9ce97.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1752090150647/c365ca6b-5676-4ffd-8616-c48a8a8bed7d.png
tags: reverseengineering-malware-staticanalysis-dynamicanalysis-sandboxing-cyberforensics, siem-qradar-microsoftsentinel-soc-loganalysis-securityanalytics-kql, cybersecurity-threathunting-malwareanalysis-blueteam-infosec-edrbypass-threatintel-mitreattack-dfir-incidentresponse

---

In today’s world of cybersecurity, malware isn’t just a buzzword; it’s an everyday reality. From ransomware attacks crippling organizations to stealthy spyware hiding in personal devices, malicious software has become one of the most common tools for attackers.

But here’s the good news: You don’t need to be a reverse engineering guru to start understanding how malware works. With the right approach, tools, and mindset, anyone in the cybersecurity field can start learning the basics of **malware analysis**.

In this blog, I’ll break down a beginner-friendly guide to analysing suspicious files even if you’re just getting started in Threat Hunting, Incident Response, or SOC operations.

## What is Malware Analysis?

Malware Analysis is the process of examining malicious software to:

* Understand how it works
    
* Identify its behaviour
    
* Detect indicators of compromise (IOCs)
    
* Develop strategies for prevention and response
    

It’s like being a digital detective, breaking down how attackers design their tools and spotting hidden dangers before they spread.

## Types of Malware Analysis

There are two main approaches to analysing malware:

### **1 Static Analysis — Examining the file without running it**

* You look at the file structure, strings, and metadata
    
* It’s safer and quicker for basic insights
    
* Great for identifying simple malware traits
    

### **2 Dynamic Analysis — Observing behaviour by executing the file in a controlled environment (sandbox)**

* You watch how the malware behaves: processes, network activity, file changes
    
* Helps uncover hidden or obfuscated behaviour
    
* Requires strict isolation to avoid infecting your system
    

As a beginner, it’s smart to start with **static analysis**, then move to dynamic techniques once you're confident.

## **Basic Static Malware Analysis: Step-by-Step**

Let’s say you’ve been handed a suspicious `.exe` file during a security investigation. Here’s how you can safely start analysing it:

### Step 1: Check the File Hashes

Always generate cryptographic hashes (MD5, SHA256) for identification.

Example tools:

**certutil -hashfile malware.exe SHA256**

**VirusTotal**

By uploading a file to VirusTotal and cross-referencing it with a list of detections from various antivirus programs, the analyst will discover whether the sample is malicious or not. This process also provides information regarding the file, such as SHA256, MD5, file size, signature info, section details, imports, etc.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752087141093/cf81c95b-8bfd-4977-bed5-d4ec13831726.png align="center")

This helps you:

* Compare against known malware databases
    
* Share samples securely with your team
    
* Maintain integrity during analysis
    

### **Step 2: Inspect the Strings**

Extracting readable text from the binary often reveals valuable clues.

Example: `strings malware.exe`

String analysis is the process of extracting readable ASCII and Unicode characters from the binary. Not all the strings found are used by the program; attackers may also include fake strings to disrupt the investigation

Tools used for string analysis:

• Strings2 – command-line utility, Windows 32bit/64bit executable, is used for extracting strings from binary data. This application is an improved version of the classic Sysinternals strings approach and can also dump strings from process address spaces. At the time of writing, Strings2 could be downloaded from the following link: [https://github.com/glmcdona/strings2](https://github.com/glmcdona/strings2)

• Flare-Floss (obfuscated string solver) - combines and automates different techniques to perform string decoding. At the time of writing, the Floss tool could be downloaded from the following link: [https://github.com/fireeye/flare-floss](https://github.com/fireeye/flare-floss)

### **Step 3: Review Metadata**

Tools like **PEStudio** or **Detect It Easy (DIE)** can reveal:

* Compiler information
    
* File type and structure
    
* Suspicious sections within the binary
    

Sometimes malware authors forget to clean up metadata, exposing their development environment or tactics.

**PEStudio**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752088201047/1927391f-1b7e-499b-a34e-2383f354f2a7.png align="center")

**PEiD Tool**

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1752088260101/3d64b35a-b994-442d-ab48-f137a3917af3.png align="center")

## **Dynamic Malware Analysis Basics**

Unlike static malware analysis, dynamic malware analysis is conducted by analysing the code while it is running. To study the behaviour of the executable, running it inside a virtual lab environment is recommended. To understand the functionality of the malware and prevent it from spreading, reverse engineers use debuggers when performing advanced dynamic malware analysis

What to monitor:

* New processes spawned
    
* Registry modifications
    
* Network connections established
    
* File system changes
    

Tools to help:

* **Procmon** — Tracks system activity
    
* **Wireshark** — Captures network traffic
    
* **Process Explorer** — Monitors running processes
    

**Important:** Never run malware on your main system. Use a dedicated, snapshot-enabled VM disconnected from your production network.

### Indicators to Watch During Analysis

Whether static or dynamic, look out for these signs:

* Suspicious outbound connections
    
* Creation of hidden files or directories
    
* Attempts to disable security tools
    
* Persistence mechanisms (autorun entries)
    

Document all findings; they’ll be critical for creating detection rules, updating threat intelligence, or informing incident response actions.

### **Final Thoughts**

Malware analysis might sound intimidating at first, but breaking it down into small, manageable steps makes it approachable for anyone.

Start small:

* Practice static analysis on harmless files
    
* Build a safe lab for observing real malware samples
    
* Stay curious — every sample tells a story
    

### **Let’s Connect**

[www.linkedin.com/in/ravi-sharma-17472316b](http://www.linkedin.com/in/ravi-sharma-17472316b)

[https://github.com/ravi160/ravi160](https://github.com/ravi160/ravi160)