---
layout: post
title: "The Phone Call After the Spam"
description: "A campaign flooding inboxes with spam then calling victims pretending to fix it. Inside: Havoc C2, DLL sideloading with 29-stage trampolines, and persistence that survives partial remediation."
date: 2026-03-20
tags: [security, social-engineering, C2, malware, ransomware, 2026]
comments: false
share: true
---

Huntress documented a campaign last month that hit five organizations. The playbook: flood a target's inbox with spam, then call them pretending to be IT support offering to fix it. The victim, already frustrated and expecting help, grants remote access. What follows is a Havoc C2 deployment with more layers of evasion than most enterprise red team engagements.

## The chain

It starts with volume. Enough spam to make the inbox unusable. Then the phone call. The attacker claims to be from IT, says they can install an "Outlook anti-spam update." The victim opens Microsoft Quick Assist or AnyDesk. From there, the attacker has hands on keyboard.

A fake Microsoft Outlook Antispam Control Panel hosted on AWS harvests the victim's credentials. Then the real payload arrives: fragmented files named `patch001.1`, `patch002.1`, reassembled into a ZIP via scripted commands. Inside the ZIP: a legitimate Adobe binary (`ADNotificationManager.exe`), several Visual C++ runtime DLLs, and an encrypted shellcode file called `license.key`.

The Adobe binary loads `vcruntime140_1.dll`, which is not the real runtime. It is a malicious loader that decrypts the shellcode using ChaCha20 and executes the Havoc Demon agent in memory. No file touches disk for the final payload.

## The evasion

The loader in `vcruntime140_1.dll` is not subtle about hiding. A 29-stage trampoline chain obfuscates control flow. Anti-emulation loops run 4 million iterations to exhaust sandbox timers. Code sections are disguised using ESET binary segments for legitimacy. The loader uses Hell's Gate to dynamically resolve syscall numbers from ntdll stubs, bypassing EDR hooks in userland. When Hell's Gate fails because a stub is hooked, it falls back to Halo's Gate, walking neighboring Nt functions and calculating the correct syscall number by offset.

ntdll hooks are installed via Detours: `RtlExitUserProcess` gets a 5-second sleep loop to prevent process termination. `LdrUnloadDll` returns immediately to block DLL unloading. The shellcode outer layer is XOR-encrypted with a 26-byte key before ChaCha20 decryption.

Huntress found three different DLL sideloading pairs across the campaign, using Adobe, Windows DLP Agent, and Windows Error Reporting binaries as hosts.

## The persistence

Within an hour of the initial compromise, the attacker deployed Havoc to four additional endpoints via scheduled tasks named with Unix epoch timestamps. Nine more endpoints were compromised over the next eleven hours.

Then the persistence diversified. Level RMM was installed on two machines. XEOX RMM on three others. No single endpoint received all three persistence mechanisms. If incident response removed one, the others survived.

## Why this matters

The technical sophistication is notable, but the entry point is what stands out. This is not a zero-day. Not a supply chain compromise. Not a watering hole. It is a phone call. A human being, overwhelmed by a deliberately created problem, accepting help from someone who sounds like they should be helping.

Huntress links the playbook to tactics previously associated with Black Basta and FIN7, though Black Basta has been reportedly dormant since late 2025 following arrests. The operators are likely former affiliates who took the tradecraft and moved on.

The lesson is old but the execution is new. The most effective initial access vector remains the one that does not require a vulnerability in software. It requires a vulnerability in a workflow. Flood the inbox. Wait for the frustration. Make the call.
