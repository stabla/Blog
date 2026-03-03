---
layout: post
title: "7,500 Building Controllers and a Vendor Who Will Not Patch"
description: "A researcher found 7,500 internet-exposed Honeywell IQ4 building controllers. 20% are accessible without authentication. Honeywell says it is not their problem."
date: 2026-03-27
tags: [security, OT, ICS, building-automation, vulnerability, 2026]
comments: false
share: true
---

Gjoko Krstic found that the Honeywell IQ4 building management controller ships with no authentication on its web interface by default. If the installing technician does not create a user account during setup, anyone who can reach the management interface gets full administrative access. Create accounts. Change settings. Control equipment.

Krstic, who runs Zero Science Lab and has a track record of over 800 building automation vulnerabilities through his Project Brainfog research, scanned the internet and found approximately 7,500 IQ4 controllers exposed to the public internet. He estimates 20% of them, roughly 1,500, can be accessed without authentication.

He reported it to Honeywell in December 2025.

## The vendor response

Honeywell says the IQ4 is designed for on-premises use and should never be internet-facing. The device ships unconfigured. Trained technicians set it up. Security is enabled automatically as part of the installation process. The vulnerability scenario can only occur during a brief installation window. They are not releasing a patch.

Krstic disagrees. He says he has personally accessed installations where no user account was created, yet he was still able to write changes to lighting, temperature, boilers, and chillers. The devices are not in some transient setup phase. They are deployed, operational, and controlling physical building systems.

He escalated to CERT/CC at Carnegie Mellon. No CVE has been assigned.

## The pattern

This is a familiar argument in OT security. A vendor designs a product for isolated networks. The product ends up on the internet. The vendor says the product is being used incorrectly. The researcher says the product is being used the way it is actually used, and that 7,500 exposed instances are evidence of a systemic deployment problem, not 7,500 individual configuration errors.

The "it is designed for on-premises use" defense shifts responsibility from the vendor to every installer, every building owner, every network administrator who ever touched the device. It assumes a level of operational discipline that the numbers prove does not exist.

## What can actually happen

The IQ4 controls HVAC systems, lighting, boilers, and chillers in commercial buildings, government facilities, and hospitals. An attacker with access to an unauthenticated controller can disable heating in a hospital wing. Overheat a server room by turning off cooling. Manipulate energy consumption. The consequences are physical, not just digital.

Broader research from Palo Alto Networks shows a 332% increase in internet-exposed OT devices in recent years. 75% of organizations have building management systems with known exploited vulnerabilities. The IQ4 is not an outlier. It is one example in a sector where the gap between intended deployment and actual deployment has been widening for years.

## The disclosure question

Krstic found the vulnerability. Reported it responsibly. Waited three months. The vendor declined to patch and disputes the impact. 1,500 controllers remain unauthenticated on the public internet. The responsible disclosure process worked exactly as designed and changed nothing for the systems that are actually exposed.

At some point, the question stops being whether the vendor should patch and starts being whether building operators should know their controllers are open. That is the tension responsible disclosure was not designed to resolve.
