---
layout: post
title: "The DNS Sinkhole That Catches What Your EDR Misses"
description: "I run a DNS sinkhole on every network I manage. It blocks C2 callbacks, phishing domains, and malware downloads at the resolver level, before the endpoint agent even sees the connection."
date: 2026-04-22
tags: [security, DNS, threat-detection, hidden-gem, 2026]
comments: false
share: true
---

Endpoint detection and response tools are good. They catch malicious processes, flag suspicious behavior, and quarantine threats on the device. I deploy them on every endpoint I manage. But EDR has a blind spot the size of a continent: it does not see devices it is not installed on.

IoT devices. Network printers. IP cameras. Building automation controllers. Smart TVs in conference rooms. Guest network devices. OT systems. Legacy servers running operating systems that no modern agent supports. Every network I have assessed has dozens or hundreds of devices that cannot run an EDR agent.

These devices still resolve DNS. And that is where a DNS sinkhole becomes the most cost-effective security control you can deploy.

## What a DNS sinkhole does

A DNS sinkhole is a recursive DNS resolver that intercepts queries to known malicious domains and returns a false response, typically 0.0.0.0 or a local IP, instead of the real address. The device trying to reach a C2 server, phishing site, or malware distribution point gets a dead-end response. The connection never establishes.

The concept is simple. The effect is profound. Every stage of most attack chains involves DNS resolution. Phishing links resolve to attacker-controlled servers. Malware payloads are downloaded from domains. C2 channels beacon to domains that rotate regularly. Data exfiltration often tunnels through DNS itself.

A sinkhole at the resolver level intercepts all of these before a single packet reaches the malicious destination. It works for every device on the network, regardless of operating system, regardless of whether an agent is installed, regardless of whether the device is managed.

## My setup

I run **Pi-hole** on every home and small-office network I manage and **Unbound** with response policy zones (RPZ) on larger deployments.

Pi-hole is the entry point for most people. It is a DNS sinkhole with a web interface, designed originally for ad blocking but equally effective as a security tool. I load it with three categories of blocklists:

**Threat intelligence feeds.** I subscribe to blocklists from abuse.ch (URLhaus, ThreatFox), Phishing.Database, the SANS Internet Storm Center, and the Disconnect malware list. These are updated multiple times per day and cover active C2 domains, phishing infrastructure, and malware distribution sites.

**Newly registered domains.** I block all domains registered in the past 48 hours. This is aggressive, and it occasionally blocks a legitimate new site. But the false positive rate is low (most new domains people need to access are not 48 hours old), and the security benefit is high. The majority of phishing infrastructure and malware C2 is hosted on domains registered within days of the attack. Blocking newly registered domains by default eliminates a massive attack surface.

**Custom blocklists.** For client networks, I maintain custom lists of domains associated with shadow IT services, unauthorized remote access tools, and known data exfiltration channels. When a policy prohibits the use of certain SaaS tools, the DNS sinkhole enforces it at the network level.

For larger environments, I run Unbound with RPZ (Response Policy Zones), which is the enterprise equivalent of Pi-hole's blocklist approach but with more granular control, logging integration, and the ability to handle higher query volumes without performance issues.

## What it catches that EDR does not

**IoT device callbacks.** I wrote about this in my passive DNS article: a smart plug on my network was beaconing to a domain hosted on bulletproof infrastructure. My EDR did not catch it because my EDR was not installed on a $8 smart plug. The DNS sinkhole blocked the callback. I saw the blocked query in the sinkhole logs before I knew the device was compromised.

**Initial phishing payloads.** When a user clicks a phishing link, the browser resolves the domain before fetching the page. If that domain is on a sinkhole blocklist, the resolution returns 0.0.0.0 and the browser shows a connection error. The phishing page never loads. The credential harvester never renders. The user sees an error and moves on. This is not a substitute for email filtering or user training, but it is an effective last line of defense that operates independently of both.

**C2 on unmanaged devices.** Guest networks, contractor laptops, BYOD phones. None of these have your EDR agent. All of them use your DNS resolver if your network is configured to enforce it. This means blocking outbound DNS (port 53 and 853) at the firewall and ideally blocking known DNS-over-HTTPS endpoints, so devices cannot bypass your resolver. Modern operating systems and browsers increasingly use DoH and DoT by default, which will circumvent a sinkhole if you do not account for it. With those controls in place, a compromised personal device on the guest network beaconing to a known C2 domain gets sinkholed the same as a managed endpoint. The device is still compromised, but the C2 channel is severed.

**DNS tunneling.** Several Pi-hole and Unbound configurations can flag or block DNS tunneling based on query characteristics: excessive subdomain length, high query volume to a single domain, unusual record types (TXT queries carrying encoded data). This detection is rudimentary compared to dedicated DNS security tools, but it catches the obvious cases at zero cost.

## The detection layer

Blocking malicious domains is the prevention layer. The detection layer is equally valuable: the sinkhole logs tell you which devices attempted to reach blocked domains.

A device that repeatedly queries a sinkholed C2 domain is likely compromised. The sinkhole prevented the C2 communication, but the device is still infected and still trying. The sinkhole log becomes an alert source.

I export sinkhole logs to my SIEM and alert on any internal device that queries a sinkholed domain more than three times in an hour. This catches compromised devices that would otherwise be invisible because they cannot communicate with their C2 and therefore exhibit no network behavior that a firewall or IDS would flag.

This inverted detection logic, finding compromises by observing blocked rather than successful connections, is one of the highest-signal alerting strategies I use.

## The cost

Pi-hole runs on a Raspberry Pi ($35-80 depending on model) or a small VM (free if you have spare capacity). The software is open source and free. The blocklists are free. The time to set up and configure is 1-2 hours. Ongoing maintenance is minimal: blocklists update automatically, and I review the logs weekly.

For enterprise deployments, Unbound with RPZ runs on any Linux server. The performance overhead is negligible. The cost is the time to integrate it into existing DNS infrastructure and configure the RPZ zones.

There is no commercial equivalent that provides this coverage for this price. Commercial DNS security services (Cisco Umbrella, Cloudflare Gateway, Zscaler) offer more features, better management interfaces, and SLA-backed uptime. But the core function, sinkholing known malicious domains, is free to implement yourself.

DNS sinkholes have been around since the mid-2000s. Not a new idea. But in my experience, fewer than 10% of the small and mid-size organizations I assess have one deployed. The ones that do typically set it up for ad blocking and do not realize they also have a security control.

The organizations that deploy sinkholes intentionally, with threat intelligence feeds, newly registered domain blocking, and log analysis, end up with a detection and prevention layer that covers every device on the network, operates independently of endpoint agents, and costs almost nothing.

Set up Pi-hole this weekend. Load the blocklists. Watch the logs. You will find something.
