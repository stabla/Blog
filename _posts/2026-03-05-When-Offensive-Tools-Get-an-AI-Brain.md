---
layout: post
title: "When Offensive Tools Get an AI Brain"
description: "An open-source AI offensive security platform was just used to compromise 600+ FortiGate appliances across 55 countries. The attacker-defender asymmetry just shifted."
date: 2026-03-05
tags: [security, AI, offensive-security, WAF, fortigate, 2026]
comments: false
share: true
---

An open-source tool called CyberStrikeAI just got caught in the wild. Built in Go, integrating over 100 security tools, powered by LLM orchestration. It was used to compromise over 600 FortiGate appliances across 55 countries in January and February.

The tool automates vulnerability discovery, builds attack chains, retrieves relevant knowledge from past exploits, and visualizes results. It was created by a developer using the alias Ed1s0nZ, who also published ransomware variants and privilege escalation detectors on their GitHub. Team Cymru traced 21 unique IPs running CyberStrikeAI during the campaign, spread across China, Singapore, Hong Kong, the US, Japan, and Switzerland.

This is not a nation-state APT framework. It is open-source. Anyone can fork it.

## The shift

Offensive security tools have always been open-source. Metasploit, Nuclei, SQLMap, Burp extensions. The difference with CyberStrikeAI is the orchestration layer. It does not just run exploits. It reasons about which ones to run, chains them together, and adapts based on what comes back. The human operator becomes optional for the reconnaissance and initial access phases.

For years, the security industry talked about AI helping defenders with triage and detection. The assumption was that attackers would need custom, expensive tooling to leverage AI offensively. That assumption died the moment someone published an AI-native attack platform on GitHub with a permissive license.

## What this means for WAF and perimeter defense

FortiGate is not some obscure appliance. It sits at the perimeter of thousands of enterprise networks. The devices that were compromised are the ones organizations trust to be the first line of defense.

When the scanning and exploitation loop is automated by an AI agent, the time between a CVE disclosure and mass exploitation compresses. Patch windows shrink. The traditional model of "patch Tuesday, exploit Wednesday" becomes "patch Tuesday, exploit Tuesday afternoon."

WAF rules and signature-based detection were designed for a world where attackers write exploits manually and reuse them. An AI-driven tool can mutate payloads, vary timing, and probe for edge cases that static rules will not catch. The defensive tooling needs to evolve at the same pace.

## The uncomfortable part

CyberStrikeAI was built for offensive security testing. Legitimate use case. But the line between a penetration testing framework and an attack tool has always been a matter of intent, not capability. Cobalt Strike taught us this years ago. The difference now is that AI lowers the skill floor. You no longer need to understand the exploit chain to execute it.

The 600 FortiGate compromises across 55 countries were not the work of a sophisticated threat actor who spent months on custom tooling. It was someone who downloaded an open-source project and pointed it at the internet.
