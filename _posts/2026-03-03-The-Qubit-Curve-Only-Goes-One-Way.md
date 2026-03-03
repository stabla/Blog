---
layout: post
title: "The Qubit Curve Only Goes One Way"
description: "RSA qubit requirements dropped from 20 million to 5,000 in seven years. The post-quantum migration window is closing faster than most organizations think."
date: 2026-03-03
tags: [security, post-quantum, cryptography, RSA, PQC, 2026]
comments: false
share: true
---

The qubit count to break RSA keeps falling. In 2019, Google estimated 20 million. By 2025, their own team revised it to under one million. This February, Iceberg Quantum published their Pinnacle Architecture claiming 98,000 physical qubits would suffice. And now the JVG algorithm, out of the Advanced Quantum Technologies Institute, claims fewer than 5,000.

That is a four order of magnitude drop in seven years. Not in qubit availability. In qubit *requirements*.

## What changed

The original threat model was straightforward: Shor's algorithm factors integers in polynomial time on a quantum computer, RSA relies on integer factorization being hard, therefore a sufficiently large quantum computer kills RSA. The catch was "sufficiently large." For RSA-2048, Shor needs millions of error-corrected qubits. We don't have those. Problem postponed.

The new approaches attack the problem differently. The JVG algorithm replaces Shor's quantum Fourier transform with a quantum number theoretic transform (QNTT), which is more noise-tolerant and gate-efficient. The paper claims a 99% reduction in total quantum gate count compared to Shor for the same factoring instances. It is a hybrid strategy: offload the heavy computation to classical machines, reserve a small, hardware-friendly task for the quantum side.

Iceberg Quantum took another path entirely. They replaced surface codes with quantum low-density parity-check (QLDPC) codes and built a modular architecture of processing units and "magic engines." Their estimate: 98,000 superconducting qubits to factor RSA-2048 in about a month. Half a million qubits to do it in a day.

## The real question

None of these results have been experimentally verified at scale. The JVG algorithm is weeks old. Iceberg's numbers come from simulations. Fair.

But the trajectory is what matters. Every year, someone finds a way to need fewer qubits. The direction is consistent and accelerating. Whether Q-Day is 2030 or 2035 is almost beside the point. The mathematical hardness assumption underneath RSA and ECC is being chipped away from multiple angles simultaneously.

NIST finalized its post-quantum cryptography standards last year. ML-KEM, ML-DSA, SLH-DSA. The migration path exists. The question is whether organizations will move before the deadline becomes a surprise.

## The uncomfortable truth

Cryptographic agility was supposed to be the answer. Swap algorithms when needed. In practice, most systems have RSA and ECC baked into layers of infrastructure that nobody fully maps. TLS certificates, VPN tunnels, code signing, firmware updates, database encryption. Each one is a migration project.

The organizations that started PQC pilots in 2024 will be fine. The ones waiting for "certainty" about quantum timelines are running a bet against a curve that only moves in one direction.

The qubits don't need to be there yet. The harvest-now-decrypt-later threat already is.
