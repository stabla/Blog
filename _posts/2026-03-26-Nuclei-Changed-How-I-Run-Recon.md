---
layout: post
title: "Nuclei Changed How I Run Recon"
description: "I used to chain five tools for vulnerability scanning. Nuclei replaced all of them. 9,000+ community templates, YAML-based, and faster than anything I have benchmarked."
date: 2026-03-26
tags: [security, tools, vulnerability, hidden-gem, 2026]
comments: false
share: true
---

Before Nuclei, my recon workflow looked like this: Nmap for port discovery, Nikto for web server checks, a custom Python script for directory brute-forcing, Burp extensions for specific vulnerability classes, and manual curl commands for everything else. Five tools, three output formats, no unified reporting, and hours of manual correlation.

I switched to Nuclei in mid-2024. Within a month I decommissioned three of those tools. Within three months I had written 40 custom templates and was running full-scope vulnerability assessments in a fraction of the time.

Nuclei is not new. ProjectDiscovery released it in 2020. But it is still the most underused tool in the average pentester's kit, and I do not understand why.

## What makes it different

Nuclei is a template-based vulnerability scanner. Every check is a YAML file that defines what to send, what to look for in the response, and how to classify the finding. The community template repository has over 9,000 templates covering CVEs, misconfigurations, default credentials, exposed panels, information disclosures, and technology fingerprinting.

The key insight is that YAML templates are readable, editable, and shareable. When a new CVE drops, someone in the community usually has a detection template published within hours. I have used Nuclei templates that were available the same day as the advisory, before commercial scanners had updated their signatures.

But the real power is in writing your own. I maintain a private repository of templates tailored to the technology stacks I encounter most frequently. Custom checks for specific CMS configurations, internal application patterns, cloud metadata endpoints, and organization-specific misconfigurations. A template takes 10-15 minutes to write and runs against every target forever after.

## Speed

I benchmarked Nuclei against my previous toolchain on a standard engagement scope: 50 subdomains, full port range, web application checks.

The old workflow took roughly 6 hours of active scanning plus 2 hours of manual correlation. Nuclei with my full template set completed in 45 minutes. The output was structured JSON that fed directly into my reporting pipeline.

Part of the speed comes from Nuclei's architecture. It handles connection pooling, rate limiting, and concurrency natively. Part comes from the template system itself. Each check is self-contained, so Nuclei can parallelize thousands of checks across hundreds of targets without the overhead of managing separate tool processes.

## Templates I use the most

**Exposed panels.** The `exposed-panels/` directory in the community templates catches admin interfaces, database consoles, monitoring dashboards, and development tools that should not be internet-facing. On nearly every external assessment I run, these templates find something. phpMyAdmin, Grafana, Jenkins, Kibana, Spring Boot Actuator. The template checks for the panel's existence and fingerprints the version in a single request.

**CVE detection.** The `cves/` directory is organized by year and maps directly to public advisories. When I am assessing a target and want to check for every known vulnerability in their technology stack, I filter templates by technology tags and run the relevant subset. This replaced my habit of manually checking "is this version of Apache affected by CVE-XXXX-YYYY" one CVE at a time.

**Misconfigurations.** CORS misconfigurations, open redirects, directory listings, verbose error pages, missing security headers. These are not glamorous findings, but they appear in every assessment and clients expect them in the report. Nuclei catches them automatically so I can focus my manual testing on application logic.

**Custom templates for cloud metadata.** I wrote a set of templates that check for SSRF paths to cloud metadata endpoints (169.254.169.254 and its equivalents), exposed AWS credentials in JavaScript bundles, and misconfigured S3 bucket policies. These are the templates that have produced the most critical findings in my engagements.

## The workflow

My current recon pipeline runs entirely on ProjectDiscovery tools. Subfinder for subdomain enumeration, Naabu for port scanning, httpx for HTTP probing and technology fingerprinting, and Nuclei for vulnerability detection. Four tools, one vendor, consistent output formats, and they integrate natively.

The command I run most often:

```
subfinder -d target.com -silent | naabu -silent | httpx -silent | nuclei -t ~/nuclei-templates/ -severity critical,high,medium -o results.json -jsonl
```

That single pipeline discovers subdomains, scans for open ports, identifies live web services, and scans them for thousands of known vulnerabilities. The output is structured JSON lines that I parse with jq or feed into my reporting tool. Start to finish, it runs unattended.

## Limits

Nuclei does not replace manual application testing. It will not find business logic flaws, authentication bypasses, or authorization issues. It is a scanner, and scanners find known patterns.

I use Nuclei to clear the ground. It handles the checklist stuff: known CVEs, common misconfigurations, the low-hanging fruit that needs to be in the report but does not require a human to find. That frees my manual testing time for logic flaws, chained vulnerabilities, and attack paths that no template can anticipate.

## Why most people underuse it

Nuclei has over 20,000 stars on GitHub. It is not obscure. But most security teams I work with either never set it up or installed it once, ran the default templates, and moved on.

The value is in sustained use. Build your template library. Customize checks for your environment. Integrate it into your CI/CD pipeline. Run it against your external attack surface on a schedule.

I have run Nuclei on every engagement for the past 18 months. It has found critical vulnerabilities on assessments where the client's commercial scanner reported clean. Community-driven templates get updated faster than commercial signature databases, and custom templates catch things generic scanners never will.

If you do security work and you are not using Nuclei, you are leaving findings on the table.
