---
layout: post
title: "The Cloud Has a Physical Address"
description: "Iranian drone strikes hit AWS data centers in the UAE and Bahrain. The first kinetic military attack on major cloud infrastructure exposes a blind spot in how we think about resilience."
date: 2026-03-08
tags: [cloud, security, AWS, infrastructure, 2026]
comments: false
share: true
---

On March 1st, Iranian drones struck two AWS data centers in the UAE and damaged a third facility in Bahrain. Fire broke out. Local authorities cut power. Sprinkler systems activated and caused water damage to servers. Sixty AWS services went offline in the ME-CENTRAL-1 region.

This is the first time a major cloud provider's infrastructure has been hit by military action. Not a simulated scenario. Not a tabletop exercise. Actual drones, actual fire, actual downtime.

## The redundancy model meets physics

AWS availability zones within a region sit within 100 kilometers of each other. That distance is designed to protect against localized failures: a power grid issue here, a network cut there. It was never designed for a state launching 540 drones and 165 ballistic missiles across a theater of operations.

Two of three availability zones in the UAE region were compromised simultaneously. The multi-AZ redundancy model, which is the foundation of every "high availability" architecture diagram ever drawn on a whiteboard, failed not because of a software bug or a configuration error but because of geography.

## Cascading failures

The downstream impact tells the real story. Abu Dhabi Commercial Bank went down. Emirates NBD went dark. Careem, the ride-hailing platform, reported a full outage. The UAE stock market closed in its first unscheduled shutdown since 2022. Major financial firms sent employees home.

These organizations did everything right by cloud standards. They deployed across availability zones. They followed the well-architected framework. The architecture was sound for every failure mode except the one that actually happened.

## The trillion dollar question

The Middle East was being positioned as a global AI infrastructure hub. Microsoft committed $15 billion to UAE cloud infrastructure. The broader Gulf investment package exceeds $2 trillion, targeting AI and chips. 326 data centers operate across the region.

The security frameworks behind these investments were built for supply chain control and political alignment. Not for protecting buildings during a military attack. That assumption is now visibly wrong.

AWS told customers to "consider taking action now to backup data and potentially migrate your workloads to alternate AWS Regions, ideally in Europe." That sentence, from the provider itself, is worth reading twice.

## What changes

The industry will likely respond with hardened facilities, underground construction, and multi-region mandates. G42 in Abu Dhabi is already building data centers across Africa, Asia, Europe, and the US as a diversification hedge.

But the deeper lesson is simpler. "The cloud" is a metaphor. The infrastructure is physical. It has coordinates. It can burn. Every disaster recovery plan that treats geographic risk as an edge case needs revision. The threat model for cloud infrastructure now includes cruise missiles, and no SLA covers acts of war.
