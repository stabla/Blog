---
layout: post
title: "AirSnitch and the Myth of Wi-Fi Client Isolation"
description: "Researchers from UC Riverside and KU Leuven broke Wi-Fi client isolation on every router they tested. Three attack techniques, zero patches available."
date: 2026-03-17
tags: [security, wifi, network-security, research, 2026]
comments: false
share: true
---

Client isolation is the Wi-Fi feature that prevents devices on the same network from talking to each other. Hotels use it. Airports use it. Every corporate guest network relies on it. The assumption is simple: even if an attacker joins the same Wi-Fi, they cannot reach your device.

Researchers from UC Riverside and KU Leuven just presented AirSnitch at NDSS 2026. Three attack techniques. Every single router they tested was vulnerable to at least one of them. Consumer gear from Netgear, TP-Link, ASUS. Enterprise equipment from Cisco and Ubiquiti. Open-source firmware. WPA2, WPA3, Passpoint. All broken.

Mathy Vanhoef, the researcher behind KRACK and FragAttacks, is one of the authors. When he publishes Wi-Fi research, it tends to be correct.

## The three techniques

**GTK abuse.** WPA2 and WPA3 networks use a shared Group Temporal Key for broadcast traffic. All clients hold this key. An attacker crafts broadcast frames encrypted with the GTK and spoofs the access point's MAC address. The victim's device accepts them as legitimate AP traffic. Client isolation logic does not inspect GTK-encrypted frames the same way it handles unicast, so the filter is bypassed at the frame level.

**Gateway bouncing.** Many routers enforce isolation only at Layer 2. The attacker sends packets to the gateway's MAC address but with the victim's IP as the destination. The gateway accepts the packet (correct MAC), routes it (correct IP), and delivers it to the victim. The gateway becomes an unwitting relay. Layer 2 isolation means nothing when Layer 3 routing ignores it.

**Port stealing.** The attacker spoofs the victim's MAC address and associates with a different BSSID on the same infrastructure. The access point's internal switching table rebinds the victim's MAC to the attacker's association. Downlink and uplink traffic get redirected. Full bidirectional interception.

## What you can do with this

Once a machine-in-the-middle position is established: intercept DNS lookups, identify visited websites, manipulate unencrypted traffic, exploit vulnerabilities in connected devices. On guest networks, cross from the guest segment into the main LAN. The isolation boundary that was supposed to prevent exactly this becomes transparent.

## The fix that does not exist

There is no CVE. Some of these issues are architectural. Client isolation was never formally specified as a security boundary with cryptographic guarantees. It is a convenience feature that everyone started treating as a security control. Wi-Fi does not cryptographically bind MAC addresses, encryption keys, and IP addresses across the network stack. AirSnitch exploits the gaps between those layers.

The researchers released an open-source toolkit on GitHub for testing. Their long-term recommendations include per-client group key randomization and filtering unicast IPs in broadcast frames. Both would require hardware-level changes from vendors.

## The practical takeaway

If you are on a public or shared Wi-Fi network and you are not running a VPN, your traffic is exposed. Not because of a new zero-day. Because the isolation feature you thought was protecting you was never designed to withstand an adversary who joins the same network and thinks about the protocol for a while.

The researchers put it directly: "Enterprises are seemingly relying on a fake sense of security." That quote applies well beyond Wi-Fi.
