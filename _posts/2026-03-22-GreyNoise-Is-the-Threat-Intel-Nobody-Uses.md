---
layout: post
title: "GreyNoise Is the Threat Intel Nobody Uses"
description: "I spent years chasing false positives from internet background noise. GreyNoise filters it out in seconds. This is the threat intel hidden gem most SOCs are missing."
date: 2026-03-22
tags: [security, threat-intelligence, hidden-gem, tools, 2026]
comments: false
share: true
---

I wasted hundreds of hours investigating alerts that turned out to be internet background noise. Mass scanners hitting every IP on the internet. Shodan probes. Censys crawlers. Research projects from universities. Botnets spraying default credentials at everything that listens on port 22.

Every SOC analyst knows the feeling. An alert fires. You pull the source IP. You check VirusTotal. It has three detections. You check AbuseIPDB. It has reports. You start building a timeline. Thirty minutes later you realize this IP is scanning the entire internet and your server is not special. It is not targeted. It is not interesting. It is noise.

GreyNoise solves this in one API call.

## What it actually does

GreyNoise maintains a network of passive sensors across the internet. When an IP hits their sensors the same way it hit yours, they tag it as background noise. Mass scanner. Benign research project. Known botnet. When you query an IP against GreyNoise, the answer is binary: this IP is scanning everyone, or it is not.

That single distinction, targeted versus opportunistic, is the most valuable triage signal I have found in ten years of security work.

I integrated GreyNoise into my SIEM correlation rules in early 2025. The effect was immediate. Alert volume on my perimeter sensors dropped by roughly 40%. Not because I was suppressing real threats. Because I was finally filtering out the noise that had been burying them.

## The free tier is enough

This is the part that surprises people. The GreyNoise Community API is free. It gives you IP lookups with classification (benign, malicious, unknown), actor name, tags, and first/last seen timestamps. For most security teams running a small to mid-size operation, this is sufficient.

I ran the free tier for six months before upgrading. During that time I used it for manual triage, automated enrichment in my SOAR playbooks, and as a pre-filter on firewall logs before they hit my SIEM. The paid tier adds historical data, bulk lookups, and the RIOT dataset which identifies known legitimate business services, but the free tier alone changed how I prioritize alerts.

## How I use it

Three integration points made the biggest difference.

**SIEM enrichment.** Every external IP that triggers an alert gets a GreyNoise lookup before it reaches an analyst. If the IP is tagged as a mass scanner with no targeted activity, the alert severity drops automatically. It still gets logged. It still gets reviewed in weekly summaries. But it does not wake anyone up at 3 AM.

**Incident response triage.** When I am investigating a potential compromise and need to determine whether inbound connections are targeted or opportunistic, GreyNoise gives me an answer in seconds. During one incident last year, a client was panicking over 200+ unique IPs hitting their VPN endpoint in a single day. GreyNoise tagged 187 of them as known mass scanners. The remaining 13 were the ones that mattered. Without that filter, the investigation would have taken days instead of hours.

**Threat hunting.** I periodically query my firewall logs against GreyNoise in bulk. Any IP that hits my infrastructure but is not in the GreyNoise dataset is interesting by definition. It means the source is not scanning everyone. It chose my network specifically. That inverted logic, focusing on what GreyNoise does not know about, is one of the most productive hunting techniques I use.

## Limits

GreyNoise does not replace a threat intelligence platform. It does not track APT campaigns, provide IOC feeds for specific malware families, or do attribution. It answers one question: is this IP doing the same thing to everyone, or just to me? Most security tools make that question hard to answer. GreyNoise makes it trivial.

I have recommended it to probably twenty security teams over the past year. Most had bookmarked it once and never set it up. Then they try it, and within a week they wonder how they operated without it.

The tool is free, the API is clean, the documentation is solid, and it plugs into every major SIEM and SOAR platform. The security industry has a bias toward complex, expensive solutions. GreyNoise does one thing and does it better than anything else I have used.
