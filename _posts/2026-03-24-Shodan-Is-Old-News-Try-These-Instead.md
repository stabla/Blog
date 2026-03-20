---
layout: post
title: "Shodan Is Old News. Try These Instead"
description: "I use Censys, FOFA, and ZoomEye more than Shodan now. Each has a different crawl strategy, different data, and different blind spots. Here is when I use which."
date: 2026-03-24
tags: [security, OSINT, tools, hidden-gem, vulnerability, 2026]
comments: false
share: true
---

Shodan was the first internet-wide scanner most security people learned about. It is still useful. But I have not used it as my primary reconnaissance tool in over a year.

The problem is not that Shodan got worse. It is that the alternatives got better, and each one sees things the others miss. Running recon against Shodan alone is like checking one camera in a building with four. You are getting a view, but not the view.

I now rotate between Censys, FOFA, ZoomEye, and Shodan depending on what I am looking for. Each has a different crawl schedule, different protocol coverage, and different data enrichment. Using all four has found exposed assets that any single one would have missed.

## Censys

Censys was built by the team behind ZMap at the University of Michigan. It crawls the entire IPv4 space and a growing portion of IPv6 on a continuous cycle. The differentiator is its certificate-based discovery. Censys indexes every TLS certificate it encounters, which means I can search for assets by certificate subject, issuer, SAN (Subject Alternative Name), or even specific certificate fingerprints.

This is how I find shadow infrastructure. A company might have 50 known domains, but their certificate transparency logs reveal 200 more. Subdomains on internal-facing services that were exposed with a valid cert. Staging environments with wildcard certificates. Legacy systems with expired certs that nobody decommissioned.

I ran Censys against a client's organization name last year and found a Kubernetes dashboard exposed to the internet on a subdomain that was not in their asset inventory. It had a valid Let's Encrypt certificate with their domain in the SAN. The dashboard had no authentication. Shodan had the IP indexed but did not surface the certificate relationship that made it findable through the organization's name.

The query language is clean. `services.tls.certificates.leaf.subject.organization: "Company Name"` gives me everything with a certificate issued to that organization. From there I filter by service type, port, software version, or geographic location.

The free tier gives 250 queries per month. For most assessments, that is enough.

## FOFA

FOFA is Chinese-developed and has the largest index I have encountered. They claim over 4 billion assets indexed. Whether or not that number is precise, the coverage is noticeably broader than Shodan's, particularly in Asia-Pacific, the Middle East, and Africa.

I started using FOFA when a client with infrastructure in Southeast Asia was not showing up completely in Shodan or Censys. FOFA had every asset, including some that had been indexed within 24 hours of deployment. The crawl frequency in those regions appears to be significantly higher than Western-focused tools.

FOFA's query syntax is different from Shodan's but equally powerful. `domain="target.com" && protocol="https"` gives me all HTTPS services on a domain. `cert.subject="Company"` does certificate-based searches similar to Censys. `header="X-Custom-Header"` lets me find assets by specific HTTP response headers, which is useful for identifying custom applications.

The interface is in Chinese by default but has an English option. The documentation is adequate. The free tier is limited but functional for spot checks. For serious use I have a paid account.

The data FOFA returns sometimes includes assets that have been offline for weeks. The freshness is inconsistent. I always verify findings with a direct probe before including them in a report.

## ZoomEye

ZoomEye is another Chinese platform, maintained by Knownsec. It has strong coverage of industrial control systems and IoT devices, which makes it my first choice when assessing OT environments or looking for exposed building automation, SCADA, or medical devices.

The differentiator is the device fingerprinting. ZoomEye categorizes devices by type (router, camera, PLC, NAS, printer) with more granularity than Shodan. When I search for `app:"Siemens S7"` on ZoomEye, the results include specific PLC model numbers, firmware versions, and module configurations that Shodan's banner grab often misses.

I used ZoomEye extensively during an OT security assessment last year. The client had 40 sites with building management systems. ZoomEye identified 6 BACnet controllers exposed to the internet that were not in the client's inventory. Three of them were running firmware with known vulnerabilities. Shodan had indexed the IPs but had not fingerprinted the BACnet service deeply enough to identify the specific controller models.

The free tier provides a limited number of monthly queries and includes API access (check their current plans for exact quotas, as they change frequently). For OT and IoT reconnaissance, it is the most cost-effective option available.

## When I use which

**Initial external attack surface mapping:** Censys. The certificate-based discovery finds assets that DNS enumeration misses. I start every engagement here.

**Broad infrastructure discovery, especially outside North America and Europe:** FOFA. The coverage in Asia-Pacific and emerging markets is unmatched.

**OT and IoT assessments:** ZoomEye. The device fingerprinting depth justifies it for any engagement involving industrial or embedded systems.

**Quick checks and scripted lookups:** Shodan. The API is the most mature, the CLI tool is convenient, and the community integrations (Maltego, Recon-ng, SpiderFoot) are extensive. For a quick "what is running on this IP?" Shodan is still the fastest answer.

**Comprehensive recon:** All four. I run the same target through each platform and merge the results. The overlap is typically 60-70%. The remaining 30-40% is split across findings that only one or two platforms caught. That delta is where the critical findings often hide.

Most security professionals I work with use Shodan exclusively. Some have heard of Censys. Almost none use FOFA or ZoomEye. The reasons are inertia and language barriers, not quality.

The internet is not uniformly visible from any single scanning platform. Different crawl schedules, different protocol parsers, different geographic vantage points mean each tool has blind spots the others cover. I found a client's exposed Jenkins instance on FOFA that had been online for three months without appearing in Shodan. I found an S7 PLC on ZoomEye that Censys had not indexed. I found certificate relationships on Censys that no other platform tracks.

If you do external reconnaissance and you only use one tool, you are missing assets. All four have free tiers. It takes minutes to check. Use them all.
