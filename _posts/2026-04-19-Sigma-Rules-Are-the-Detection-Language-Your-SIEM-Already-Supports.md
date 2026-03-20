---
layout: post
title: "Sigma Rules Are the Detection Language Your SIEM Already Supports"
description: "I stopped writing vendor-specific detection rules two years ago. Sigma lets me write once and deploy to Splunk, Elastic, and Sentinel. My detection library is finally portable."
date: 2026-04-19
tags: [security, detection, hidden-gem, tools, 2026]
comments: false
share: true
---

I have worked with three different SIEM platforms in the past five years. Each time I migrated, I rewrote my detection rules from scratch. Splunk SPL to Elasticsearch Lucene queries. Then to Microsoft Sentinel's Kusto Query Language (KQL), which is an entirely different language despite Kibana also having something called KQL. Hundreds of hours translating the same logic into different query languages.

In 2024, I discovered Sigma, and I stopped writing vendor-specific detection rules entirely.

Sigma is a generic, open signature format for SIEM detections. You write a rule once in YAML. A converter translates it into the query language of whatever SIEM you use. Splunk, Elastic, Microsoft Sentinel, QRadar, CrowdStrike LogScale, Chronicle, Grafana Loki. One rule, every platform.

It is YARA for logs. And like YARA, once you start using it, you cannot imagine working without it.

## The format

A Sigma rule is a YAML file with a standard structure: title, description, log source, detection logic, and metadata (severity, MITRE ATT&CK mapping, false positive notes). The detection logic uses a simple, readable syntax.

```yaml
title: Suspicious PowerShell Download Cradle
status: test
description: Detects PowerShell download cradle patterns commonly used for malware delivery
date: 2026/03/01
logsource:
    category: process_creation
    product: windows
detection:
    selection:
        CommandLine|contains|all:
            - 'powershell'
            - 'IEX'
            - 'Net.WebClient'
    condition: selection
level: high
tags:
    - attack.execution
    - attack.t1059.001
```

That rule detects a PowerShell download cradle on any SIEM that ingests Windows process creation events. I write it once. The Sigma converter (pySigma or the sigma-cli tool) translates it to Splunk SPL, Elasticsearch Lucene/DSL, Sentinel KQL, or whatever backend I need. No SIEM runs Sigma natively. The converter is a required step. But the point is that I write the logic once and the translation is automated.

When I changed jobs and moved from Splunk to Elastic, I converted my entire rule library in an afternoon. No rewriting. No logic errors from manual translation. The rules just worked.

## The community library

The SigmaHQ repository on GitHub contains over 3,000 detection rules maintained by the community. These cover Windows, Linux, macOS, cloud platforms (AWS, Azure, GCP), web proxies, firewalls, and application logs.

The coverage is comprehensive and current. When a new threat technique is documented, Sigma rules for detection often appear within days. The LAPSUS$ TTPs, the MOVEit exploitation, the Citrix Bleed attacks, all had community Sigma rules published faster than most commercial detection content providers updated their rule packs.

I use the community library as a baseline and customize from there. Roughly 60% of my deployed detections started as community Sigma rules that I tuned for my environment: adjusting thresholds, excluding known-good paths, and adding organization-specific context.

## How I use it in practice

**Detection-as-code.** My Sigma rules live in a Git repository. Changes go through pull requests with peer review. Each merge triggers a CI pipeline that validates the YAML syntax, runs the Sigma converter, and deploys the translated rules to the SIEM. This is infrastructure-as-code applied to security detections. Every rule has history, attribution, and a review trail.

**Portable detection library.** I maintain about 400 custom Sigma rules covering the threat landscape relevant to my clients. This library travels with me. When I onboard a new client on a different SIEM platform, I convert and deploy my library in hours. The intellectual investment in detection logic is preserved across platforms.

**MITRE ATT&CK mapping.** Every Sigma rule includes ATT&CK tags. This means I can generate a coverage heat map showing which techniques I detect and which I do not. Gaps become visible. When a red team report identifies a technique that bypassed detection, I check whether I have a Sigma rule for it, and if not, I write one. The ATT&CK integration turns my detection library into a measurable capability.

**Sharing with clients.** When I write a custom detection for a client engagement, I deliver it as a Sigma rule with the converted SIEM query. If the client later migrates to a different platform, the rule migrates with them. This is better for the client and better for my reputation. Nobody wants a consultant whose deliverables are locked to a specific vendor.

## Limits

Sigma is a detection logic format, not a detection engine. It does not run queries, manage alerting, or handle response. It translates human-readable YAML into whatever query language your SIEM speaks.

The converters are not perfect. Complex detection logic sometimes translates with edge cases that need manual tuning. Aggregation queries (count, group by, timespan) behave differently across backends, and I always test converted rules against sample data before deploying to production.

Sigma also does not replace the need to understand your data. A rule that detects PowerShell download cradles is useless if your SIEM does not ingest process creation events with full command-line logging. Sigma tells you what to look for. You still need to make sure the data exists.

## Why nobody uses it properly

Sigma has been around since 2017. The SigmaHQ repository has over 8,000 stars on GitHub. It is not unknown. But the majority of security teams I work with either do not use it at all or use it superficially: running the occasional community rule without building their detection program around it.

The teams that go all in on Sigma, treating it as the foundation of a detection-as-code workflow, end up with portable, reviewable, testable detection libraries that survive platform migrations. The teams that write vendor-specific queries directly into their SIEM end up with detection logic locked in a platform they may not use in three years.

I regret every hour I spent writing raw SPL queries that I later had to rewrite in Kusto. Sigma would have saved all of that. If you write detection rules for any SIEM, start here.
