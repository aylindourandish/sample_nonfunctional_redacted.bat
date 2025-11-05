# Redacted Windows Batch — Defensive Study
> a small, safe artifact for learning — made by a student

Hey — I’m sharing a redacted snippet I analyzed for a course assignment.  
This repo does not contain runnable malware. I removed (commented out)
all commands that would change a real machine and replaced any private
values with <REDACTED_...>. The goal here is academic: learn attacker
techniques and how to detect them, without putting anyone or any system
at risk.

Note: I secured and redacted this sample *with the help of an AI assistant* so it would be safe to share. The edits were made to reduce risk and make the file suitable for classroom review. The final text has been written and human-reviewed to keep a natural, student-friendly tone.

---

## TL;DR (short and plain)
- This is a non-functional, redacted Windows batch file for study.
- The original script tried to: gain admin, disable Defender, create an admin user, enable RDP, and change firewall/registry settings.
- Everything dangerous is commented out. Don’t run any of the commands in the original form.
- Use this repo for static review, discussion, and building detection rules — _not_ for execution.

---

## Why I made this
I had to analyze a suspicious Windows batch for a class project. I wanted to:
1. Understand what the script did (privilege escalation, persistence, defense evasion).  
2. Share a safe, readable artifact so classmates and instructors can review it.  
3. Provide clear indicators and practical defensive tips so defenders can spot similar attacks.

Because publishing working malware is dangerous and irresponsible, I redacted the harmful parts and documented what they would have done. I used an AI assistant to help with safe redaction and to draft clearer, more teacher-friendly explanations — then I reviewed and edited everything by hand so the tone stays natural and easy to read.

---

## What the original (redacted) script attempted
(All actions below are descriptions — the commands are commented out in the sample.)

- Relaunch itself with administrative privileges (used a tiny VBScript helper).
- Turn off Windows Defender’s real-time protection.
- Create a local user and add it to the Administrators group.
- Modify registry key fDenyTSConnections to enable Remote Desktop.
- Add/enable firewall rules for Remote Desktop.
- Stop and disable the Defender service and alter Defender-related registry keys.
- Turn off Windows Firewall entirely.

Those behaviors are classic post-exploitation / persistence techniques. We study them to learn how to detect and mitigate.

---

## How to use this safely (please follow!)
1. Do not run any .bat file from this repo on production or personal devices.  
2. For hands-on study, use an isolated VM (snapshot, no network or a controlled lab network).  
3. Prefer static analysis — read the file, search for IoCs, and create detection rules.  
4. If you need to reproduce behavior for research, get explicit written permission and teacher / lab-admin oversight.

---

## Quick detection & hunting checklist (practical)
If you are defending a Windows environment, look for these signs:

Command lines / PowerShell  
- Set-MpPreference -DisableRealtimeMonitoring  
- reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows Defender"  
- sc stop WinDefend or sc config WinDefend start= disabled  
- net user <username> <password> followed by net localgroup administrators

Registry keys to monitor  
- HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\fDenyTSConnections  
- HKLM\SOFTWARE\Policies\Microsoft\Windows Defender\*
- Events to alert on  
- Security Event ID 4720 (user account created)  
- 4732/4733 (group membership changed)  
- 7040/7036 (service changed/stopped)  
- Process creation events showing the commands above

Network/Firewall  
- Sudden RDP (3389) openings from previously unseen sources  
- Firewall rule adds/changes (netsh advfirewall command lines)

---

## A small note for non-technical reviewers (e.g., instructors)
If you’re not a security person and want to understand the impact quickly:
- The script aimed to give an attacker persistent admin access and remove local protections.  
- That makes cleanup and recovery much harder: the attacker could re-create accounts, re-enable backdoors, or remove detection tools.  
- The right reaction: isolate the host, collect forensic evidence (event logs, registry hives, scheduled tasks), and restore from known-good backups where feasible.

---

## Ethics & legal stuff (please read)
This repository is for defensive education only.  
Sharing or using real malware without permission is illegal and unethical. If you’re doing research, always operate in an approved lab with recorded permission.


Thanks for reading. Stay safe, and always run experiments in a properly isolated lab.
