---
layout: post
title: "NIS2 Compliance Starts with Asset Inventory, Not Policies"
description: "I have reviewed a dozen NIS2 readiness programs. They all start with writing policies. They should start with knowing what they have. You cannot protect assets you have not inventoried."
date: 2026-04-08
tags: [security, NIS2, GRC, compliance, hidden-gem, 2026]
comments: false
share: true
---

Every NIS2 readiness program I have reviewed in the past year starts the same way. A consulting firm comes in. They do a gap analysis against the directive's requirements. They produce a list of policies that need to be written or updated. The organization spends six months drafting incident response policies, risk management frameworks, supply chain security procedures, and board-level reporting templates.

Then someone asks: "What systems are actually in scope?"

And nobody has a complete answer.

This is the pattern I see repeatedly. Organizations invest heavily in the governance layer, policies, procedures, and documentation, while the foundational question of "what do we actually need to protect?" remains unanswered. NIS2 compliance built on an incomplete asset inventory is compliance theater. The policies are correct. The scope is wrong. And when an incident occurs, the response plan references systems that nobody knew existed.

## Why asset inventory is the real starting point

NIS2 Article 21 requires "essential" and "important" entities to implement cybersecurity risk management measures that are proportionate to the risk. But proportionate to the risk of what? You cannot assess risk on systems you do not know about. You cannot apply security measures to infrastructure that is not in your inventory. You cannot report incidents affecting services you have not mapped.

The directive's scope determination itself depends on knowing what you operate. NIS2 applies to organizations based on the services they provide and their size. An organization might assume it operates only one service in scope, but a complete asset inventory might reveal dependencies, shared infrastructure, or secondary services that bring additional systems under the directive's requirements.

I worked with a logistics company last year that believed its NIS2 scope was limited to its transport management system. An asset inventory discovered that the transport management system depended on an internally hosted DNS resolver, a legacy ERP system for customs documentation, three cloud-based APIs for carrier integration, and a building management system that controlled access to the data center. All of these became in-scope infrastructure because they supported the essential service. The original gap analysis, done without an asset inventory, had missed every one of them.

## What a real asset inventory looks like

The asset inventories I see most often are spreadsheets maintained by IT. They list servers by hostname, maybe an IP address, maybe an owner. They are updated manually, usually when someone remembers. They are incomplete on the day they are created and increasingly wrong every day after.

A useful asset inventory for NIS2 purposes needs to capture:

**Network assets.** Every device with an IP address on every network segment. Servers, workstations, network equipment, printers, IoT devices, OT controllers, building automation endpoints. Automated discovery tools (Rumble/runZero, Lansweeper, or even regular Nmap scans) are the minimum. Manual inventories will always miss the device someone plugged in six months ago and forgot about.

**Software assets.** Every application, service, and middleware running on those devices. This includes operating systems, installed packages, running services, and their versions. This is where vulnerability management connects to asset management. You cannot patch what you do not know is installed.

**Cloud assets.** Every cloud account, subscription, and resource across every provider. Shadow IT cloud deployments are common and almost never appear in manual inventories. I recommend running cloud security posture management tools (CSPM) or at minimum using each provider's native inventory APIs (AWS Config, Azure Resource Graph, GCP Cloud Asset Inventory).

**Data assets.** Where sensitive data is stored, processed, and transmitted. NIS2 requires notification of significant incidents, defined as those causing severe operational disruption, financial loss, or considerable damage to other persons. Knowing where your data lives determines what constitutes a significant incident and who is affected.

**Dependencies.** How systems connect to each other. The logistics company's transport management system was the obvious in-scope asset. Its dependencies on DNS, ERP, APIs, and physical access control were the non-obvious scope expansion. Dependency mapping turns a flat list of assets into a service architecture that accurately represents what needs protection.

## The tools I recommend

For network discovery, I use **runZero** (formerly Rumble). It does unauthenticated network scanning that fingerprints devices with surprising accuracy. I have deployed it at several client sites and it consistently finds 15-30% more assets than the client's existing inventory. The agent-less approach means it discovers devices that endpoint agents miss: IoT, OT, network appliances, and rogue devices.

For software inventory, **Snipe-IT** is an open-source asset management platform that is good enough for organizations that do not want to pay for ServiceNow or Lansweeper. It is not automated discovery, but it provides a structured database that is infinitely better than a spreadsheet.

For cloud inventory, the native provider tools are sufficient if you actually use them. **AWS Config** with aggregation across all accounts, **Azure Resource Graph** queries, and **GCP Cloud Asset Inventory** each provide a complete view of their respective platforms. The challenge is usually organizational: nobody has aggregated all cloud accounts into a single inventory view.

For dependency mapping, I have not found a single tool that does it well across all environments. I typically combine application performance monitoring data (Datadog, New Relic), network flow data (NetFlow/sFlow analysis), and manual interviews with system owners. It is the hardest part of the inventory and the most valuable.

## The compliance connection

Once you have a real asset inventory, the NIS2 compliance work gets much easier.

**Risk assessment** is meaningful because you know what you are assessing. Instead of generic risk statements about "IT systems," you are evaluating specific risks to specific assets with known dependencies.

**Incident response** is actionable because you know what systems support which services. When an incident occurs, you can immediately determine scope, impact, and notification requirements.

**Supply chain security** is traceable because you know which third-party components are in your environment. NIS2 Article 21(2)(d) requires supply chain risk management. That starts with knowing what your supply chain actually is.

**Board reporting** is credible because it is based on measured coverage, not assumptions. "We have security controls on 94% of our inventoried assets" is more useful to a board than "we have policies in place."

## Do it first

Every security framework lists asset inventory as a foundational control. The part nobody follows through on is doing it before everything else.

I tell every organization starting NIS2 compliance to spend the first 4-6 weeks exclusively on asset discovery and dependency mapping. No policies. No procedures. Just find out what you have, where it is, what it does, and what depends on it.

The results will reshape every compliance activity that follows. The scope will be different from what you assumed. The risks will be different from what the generic framework suggested. The priorities will be different from what the consulting firm recommended without this data.

Start with the inventory. Everything else follows.
