---
layout: post
title: "Passive DNS Is the Log You Are Not Collecting"
description: "I added passive DNS logging to my network and found a compromised IoT device within 48 hours. Most organizations have no idea what their DNS traffic reveals."
date: 2026-03-30
tags: [security, DNS, threat-detection, hidden-gem, 2026]
comments: false
share: true
---

DNS is the most underutilized data source in most security operations. Every device, every application, every malware implant on your network resolves domain names before it communicates. The DNS query log is a near-complete record of intent: what every host on your network wanted to talk to, when, and how often.

Most organizations do not collect this data. Their DNS resolvers run on defaults. Queries are resolved and forgotten. No logs. No retention. No analysis. They have firewalls, endpoint detection, SIEM, and a dozen other security tools, but they are blind to the single protocol that almost every threat must use.

I added passive DNS logging to my home network in 2024 and to my clients' environments throughout 2025. It has been the single highest-signal detection source I have deployed.

## The 48-hour discovery

Two days after enabling DNS logging on my home network, I found a smart plug resolving a domain registered three days prior, hosted on a known bulletproof hosting provider. The device was a no-name smart plug I had bought for $8 on Amazon. It was sending DNS queries to a domain that had no legitimate purpose. The query pattern was consistent with a beacon: one resolution every 15 minutes, like clockwork.

I blocked the domain, isolated the device, and captured the traffic. The plug was exfiltrating local network topology data to an endpoint in Eastern Europe. Nothing catastrophic. No credentials stolen, no lateral movement. But without DNS logging, I would never have known.

This is the reality of IoT devices on residential and corporate networks. They resolve domains you have never heard of, on schedules you did not authorize, to destinations you cannot verify. DNS logging makes this visible.

## What passive DNS reveals

**Beaconing.** Command-and-control channels typically resolve their server's domain at regular intervals. A host that queries the same unusual domain every 60 seconds, 5 minutes, or 15 minutes is exhibiting beacon behavior. This pattern is almost invisible in firewall logs but trivially detectable in DNS data.

**Domain generation algorithms.** Many malware families use DGAs to generate pseudo-random domain names, ensuring their C2 infrastructure is resilient to takedowns. DGA domains have distinctive characteristics: high entropy, unusual TLDs, and sudden bursts of NXDOMAIN responses. A DNS log makes DGA activity obvious.

**Data exfiltration.** DNS tunneling encodes data in subdomain labels. A query to `aGVsbG8gd29ybGQ.evil.com` is not a legitimate lookup. It is data being smuggled out through the DNS protocol, which most firewalls allow unconditionally. Passive DNS logging catches this because the query lengths and entropy are anomalous.

**Shadow IT.** Employees installing unauthorized SaaS tools, personal VPNs, or remote access software. All of it shows up in DNS. When a workstation starts resolving `*.ngrok.io` or `*.tailscale.com` and those services are not sanctioned, you want to know.

**Newly registered domains.** Phishing infrastructure, malware C2, and credential harvesting sites are almost always on domains registered within the past 30 days. Cross-referencing DNS queries against domain age is one of the most reliable heuristics for catching threats early. If a device on your network is talking to a domain that did not exist last week, that deserves investigation.

## How I set it up

For my home network, I run Unbound as my recursive resolver with logging enabled. Queries go to a local syslog instance, which I parse with a simple Python script that flags anomalies: high-entropy domains, domains younger than 30 days (checked against WHOIS), excessive NXDOMAIN responses, and regular-interval query patterns.

The total cost was zero. Unbound is open source. The parsing script was 200 lines of Python. The hardware is a Raspberry Pi I already had.

For client environments, the approach scales differently depending on infrastructure. Organizations running Active Directory already have DNS servers that can enable logging. The challenge is storage and analysis, not collection. Windows DNS debug logging generates substantial volume. I typically ship it to the SIEM with pre-filters that drop known-good domains (CDNs, Microsoft 365, Google Workspace) and retain everything else for analysis.

For cloud-native environments, AWS Route 53 Resolver query logging and Google Cloud DNS logging provide equivalent data without deploying anything. The logs flow into CloudWatch or Cloud Logging and can be analyzed with standard tools.

## The tools that matter

**DNS Dumpster and SecurityTrails** for historical passive DNS data. When I find a suspicious domain in my logs, I check its resolution history. A domain that pointed to ten different IPs in the past month is more suspicious than one with a stable resolution.

**CIRCL Passive DNS** for community-sourced resolution data. When I want to know if other networks are seeing the same domain, CIRCL provides crowd-sourced visibility.

**Zeek (formerly Bro)** for network-level DNS monitoring on larger deployments. Zeek parses DNS traffic passively and generates structured logs that feed directly into SIEM platforms.

**Pi-hole or AdGuard Home** for home users who want DNS logging without building a custom setup. Both tools log every query by default and have built-in dashboards. The security value is a side effect of their ad-blocking purpose, but it is significant.

## Almost nobody does this

I have audited the security posture of dozens of small and mid-size organizations. Fewer than 20% had any form of DNS logging enabled. Fewer than 5% were actively analyzing DNS data for threats.

DNS logging is free, trivial to enable, and catches threats that no other data source reliably detects. Endpoint agents miss IoT devices. Firewalls do not inspect DNS content. Network detection tools often skip port 53 in their analysis.

If something on your network is compromised, it will eventually make a DNS query to a domain the attacker controls. If you are not logging those queries, you are missing the one signal that almost every attacker has to generate.

Start collecting DNS logs. Today. You will find something within a week.
