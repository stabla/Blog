---
layout: post
title: "Your Tires Are Broadcasting"
description: "Tire pressure sensors transmit a unique 32-bit ID in cleartext on every car built since 2007. Researchers tracked 20,000 vehicles with $100 radios."
date: 2026-03-25
tags: [security, privacy, IoT, vehicle, tracking, 2026]
comments: false
share: true
---

Every car manufactured since 2007 in the US and 2014 in Europe is legally required to have tire pressure monitoring sensors. Each sensor contains a battery, a pressure gauge, a temperature gauge, and a radio transmitter. It broadcasts on 315 MHz (North America) or 433 MHz (Europe), in cleartext, with no encryption, no authentication, and no obfuscation. Every transmission includes a unique 32-bit sensor ID that never changes.

Researchers from IMDEA Networks Institute in Madrid deployed a network of $100 software-defined radio receivers near roads and parking areas. Over 10 weeks they collected 6 million TPMS messages from more than 20,000 vehicles. Published at IEEE WONS 2026.

## What leaks

Each tire has its own sensor with its own static ID. A car has four. When the same four IDs consistently appear together, they form a fingerprint. 128 bits of combined uniqueness, more reliable than a license plate because it works through walls, at distances exceeding 50 meters, and the target has no indication they are being observed.

The sensors transmit pressure, temperature, and status flags. Pressure readings reveal whether the vehicle has been recently driven and roughly how heavy the load is. Timing patterns reveal when a car arrives and leaves. Over time, this builds a profile: work schedules, remote-work days, routine routes, when a house is unoccupied.

## Why this is different from other tracking methods

License plate readers require line of sight, cameras, and visible infrastructure. Cell tower triangulation requires cooperation from carriers or law enforcement access. Bluetooth and Wi-Fi tracking have prompted manufacturers to implement MAC address randomization.

TPMS has none of these constraints. The receiver is passive. It emits nothing. There is nothing for the target to detect. A network of receivers covering a small city would cost less than a single ALPR camera installation. The signals penetrate structures. The identifiers never rotate.

And unlike Bluetooth or Wi-Fi, which device manufacturers have started to address with randomization, the automotive industry has done nothing. The vulnerability was first documented in 2010 at USENIX Security. Schneier wrote about it in 2016. In 2024, Synacktiv demonstrated a zero-click remote code execution on a Tesla Model 3 through its TPMS subsystem. The 2026 research simply proved, at scale, what has been known for sixteen years.

## No fix exists

The sensors are constrained hardware. Tiny batteries designed to last 7 to 10 years. Minimal processing power. Retrofitting encryption or ID rotation onto the existing installed base is not possible. Any fix requires new sensor hardware in new vehicles, meaning every car currently on the road will continue broadcasting indefinitely.

The researchers recommend encrypted transmissions, rotating identifiers, and regulatory action to mandate TPMS privacy protections. No manufacturer has implemented any of these in production.

Governments mandated these sensors for safety. They did not mandate security for the sensors themselves. The result is a legally required surveillance surface installed in hundreds of millions of vehicles, broadcasting persistent identifiers that anyone with a $100 radio can receive.
