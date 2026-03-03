---
layout: post
title: "Your Hosting Panel Is Already For Sale"
description: "Flare analyzed 200,000 underground posts in a single week. 90% were selling compromised cPanel credentials as commodity phishing infrastructure."
date: 2026-03-14
tags: [security, cybercrime, phishing, hosting, cPanel, 2026]
comments: false
share: true
---

Flare published research last week based on 200,000 underground posts collected in a seven-day window. The subject: compromised site management panels. Mostly cPanel, some Plesk, some custom dashboards. All for sale. Bulk pricing available.

90% of those posts were duplicates. Not because the data is stale, but because the market is so structured that individual listings get amplified across hundreds of Telegram channels and forums. It operates like a supply chain. Harvesters on one end, resellers in the middle, phishing operators on the other.

## What is being sold

A compromised cPanel account gives you full backend control of a web hosting environment. File management, database access, user accounts, security settings, email configuration. If the account includes a working SMTP server, the price goes up, because the buyer can send phishing emails from a legitimate domain with a clean reputation. No need to set up infrastructure from scratch. No need to warm up an IP. The domain is already trusted.

Prices range from a few dollars per credential in bulk to several thousand for high-value panels: US or EU domains with clean reputation, active SMTP, high traffic, or access to payment processing.

## How they get compromised

Credential stuffing is the most common vector. Password reuse from prior breaches. Automated brute-force against exposed cPanel login pages. Infostealer malware, which Flare estimates harvested 1.8 billion credentials in 2025 alone. Botnets continuously scanning for exposed panels, known CVEs, and misconfigurations.

Because the access comes through valid credentials, traditional security monitoring often misses it entirely. There is no exploit to detect. No anomalous binary to flag. Someone logs in with the right password and starts uploading phishing kits.

## The economics

This is the part worth thinking about. The cybercrime economy has quietly shifted from exploit development to access brokerage. Writing a zero-day requires skill. Buying a compromised cPanel for $5 requires a Telegram account and a cryptocurrency wallet.

The barrier to running a phishing campaign used to include standing up infrastructure: registering domains, configuring DNS, setting up mail servers, warming up IPs to avoid spam filters. All of that friction is gone when you buy a cPanel with an established domain. The infrastructure is pre-built, pre-trusted, and pre-warmed.

Hosting providers now face a problem that traditional security tools were not designed for. The compromise happens through the front door. The malicious activity looks like normal cPanel usage. The phishing pages are uploaded through the same file manager that legitimate users access. The spam emails go through the same SMTP server that handles real business correspondence.

## What this means for site owners

If your cPanel credentials appeared in a stealer log and you reused that password, your hosting account might already be listed on a Telegram channel. The signs show up downstream: search engine ranking drops, domain blacklisting, customer complaints about phishing emails from your domain. By the time those signals arrive, the damage is done.

The mitigation is straightforward but rarely implemented at scale: unique passwords on hosting panels, two-factor authentication where available, regular audits of file changes and email sending patterns. The boring stuff. The stuff that prevents your domain from becoming someone else's inventory.
