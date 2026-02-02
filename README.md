# MemLabs Lab 1 – Beginner’s Luck
## Memory Forensics Case Study (DFIR)
### Overview

This case study documents my investigation of MemLabs Lab 1, a beginner-level memory forensics challenge designed to simulate a real-world incident involving a Windows system crash.

The objective was to recover three important artifacts (“flags”) from a raw memory dump.

The scenario stated:

-A Windows system crashed unexpectedly.

-A black command window briefly appeared.

-The user was drawing something at the time of the crash.

Only a memory image was provided — no disk access.

The investigation was performed entirely through volatile memory analysis.

### Goals

Validate memory dump integrity

Identify system profile and runtime context

Reconstruct user activity from RAM

Recover hidden artifacts

Demonstrate forensic methodology

Integrate AI safely to accelerate analysis

### Tools Used
-Core DFIR

-Volatility 3

-DumpIt (present in memory)

-GIMP (image reconstruction)

-CyberChef (decoding)

-WinRAR / KeePass (artifact access)

### Supporting Utilities

-strings

-foremost / scalpel

-hash utilities

### AI Assistance

Gemini CLI (“Athena”) — used as a supervised forensic assistant

AI was not allowed to decide investigative direction.
It was used strictly for:

-running approved Volatility commands

-organizing extracted artifacts

-reconstructing fragmented text

-accelerating repetitive tasks

All pivots and conclusions were analyst-driven.

### Environment Identified

-OS: Windows 7 SP1 x64

-Single CPU system

### User accounts discovered:

-Alissa Simpson

-SmartNet

Memory integrity verified via MD5 before analysis.

### Investigation Methodology
1. Initial Profiling

Using windows.info:

Confirmed Windows version

Identified kernel base

Established system timestamp

Process listing (pslist) revealed key activity:

-cmd.exe / conhost.exe → black window explanation

-mspaint.exe → drawing activity

-WinRAR.exe → archive creation

-DumpIt.exe → memory acquisition

These processes became primary pivots.

2. Flag 1 – UserAssist Registry + Batch File

UserAssist registry artifacts showed execution of:

'C:\Users\SmartNet\Desktop\st4G3$$1.bat'


File was located in memory and dumped.

Contents:

'ECHO ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0='


Decoded Base64 → first flag recovered.

Key takeaway:
Registry artifacts persist even when files attempt to hide.

3. Flag 3 – WinRAR + NTLM Hash Pivot

Command-line analysis revealed:

WinRAR.exe Important.rar


The archive contained a hint:

Password is NTLM hash (uppercase) of Alissa’s account

Using hashdump, the NTLM hash was extracted and converted to uppercase.

Archive successfully decrypted, revealing an image containing Flag 3.

Key takeaway:
Credential material often survives in memory long after execution.

4. Flag 2 – MSPaint Process Memory Reconstruction

No saved image file existed.

The flag lived only inside volatile MSPaint canvas memory.

Approach:

-Dumped mspaint.exe

-Converted memory to raw format

-Loaded into GIMP

A-djusted dimensions until handwritten content appeared

Flag 2 recovered from reconstructed drawing.

Key takeaway:
Application memory may contain unsaved user data unavailable anywhere else.

AI Integration

Gemini CLI (“Athena”) was introduced mid-investigation.

Usage model:

-Analyst defines objective

-Analyst approves commands

-AI executes Volatility or parsing tasks

Results reviewed manually

Benefits observed:

-Faster artifact extraction

-Cleaner reconstruction of fragmented strings

-Reduced repetitive workload

AI did not replace forensic reasoning — it enhanced analyst efficiency.

This reflects real-world DFIR workflows where AI supports humans, not replaces them.

### Results

All three flags recovered using memory-only analysis:

-Flag 1: Hidden batch file (UserAssist + Base64)

-Flag 2: Unsaved MSPaint canvas (process memory reconstruction)

-Flag 3: Password-protected RAR (NTLM hash pivot)

### Lessons Learned

-Memory holds far more than most attackers expect.

-UserAssist remains a powerful execution artifact.

-Application RAM can expose unsaved sensitive content.

-Credentials frequently linger in volatile memory.

-AI dramatically improves speed when used responsibly.

-Forensics is about correlation, not tools.

### Next Steps

This lab is part of a multi-stage DFIR learning series.

Moving forward to MemLabs Lab 2, which increases complexity by introducing:

-Browser artifacts

-Password managers

-External downloads

-Multi-source correlation

### Author

Geoffrey Muriuki Mwangi
DFIR / SOC Analyst
Hands-on memory forensics practitioner
