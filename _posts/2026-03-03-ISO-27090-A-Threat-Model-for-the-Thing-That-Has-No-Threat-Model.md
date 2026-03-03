---
layout: post
title: "ISO 27090: A Threat Model for the Thing That Has No Threat Model"
description: "The first international standard for cybersecurity threats to AI systems maps 13 attack categories across the AI lifecycle. It fills a gap that most organizations do not know they have."
date: 2026-03-03
tags: [security, AI, ISO, GRC, standards, 2026]
comments: false
share: true
---

Most organizations deploying AI systems today have no structured way to think about the security threats specific to those systems. They have firewalls. They have endpoint detection. They have ISO 27001 controls covering traditional information security. None of it addresses prompt injection, data poisoning, or model theft in any systematic way.

ISO/IEC 27090 is the first international standard that tries to fix this. It passed its Draft International Standard vote in July 2025 and is expected to reach final publication in mid-2026. It is a guidance document, not a certifiable management system. You cannot get audited against it. But it provides something that did not previously exist in standardized form: a structured taxonomy of 13 AI-specific threat categories with causes, consequences, detection methods, and mitigation strategies for each.

## What it covers

The standard maps threats across the entire AI lifecycle: development, training, deployment, and operation. The 13 threat categories include the ones that keep AI security researchers busy:

**Data poisoning.** Manipulating training data to influence model behavior, degrade performance, or plant backdoors. The mitigation guidance covers data provenance, integrity validation, and anomaly detection in training pipelines.

**Evasion attacks.** Adversarial inputs crafted to make deployed models misclassify or produce incorrect outputs. The classic example: pixel-level perturbations that make a stop sign invisible to a vision system.

**Prompt injection.** Manipulation of LLM inputs to override system instructions or extract information the system was not designed to reveal. This is the threat category that most organizations deploying LLMs have encountered firsthand, usually without a formal framework for thinking about it.

**Model inversion and membership inference.** Attacks that reconstruct training data from model outputs or determine whether specific records were in the training set. These are privacy-security intersections that traditional infosec controls do not address.

**Model theft.** Extraction or replication of the intellectual property embedded in a trained model. This matters to organizations that have invested significant compute and proprietary data into their models.

**AI supply chain attacks.** Targeting pre-trained models, open-source datasets, and ML libraries. The AI supply chain is less mature and less audited than the software supply chain, and ISO 27090 explicitly calls this out.

## Where it fits

ISO 27090 sits in the ISO 27000 family alongside ISO 27001 (the ISMS standard) and ISO 27002 (the code of practice for security controls). The relationship is intentional. Organizations with existing 27001 certifications can use 27090 to identify the AI-specific gaps in their risk assessments that traditional controls do not cover.

It also complements ISO 42001, which is the certifiable AI Management System standard. Where 42001 tells you how to govern AI systems, 27090 tells you what to defend them against. Different tools, same shelf.

In the regulatory landscape, ISO 27090 sits alongside the EU AI Act, NIST AI RMF, and the emerging European harmonized standards (ETSI, CEN-CENELEC). It has no formal legal connection to any of them. Its advantage is jurisdictional neutrality: it works the same whether you operate under EU, US, or Asian regulatory frameworks.

## Who needs this

Any organization that develops or deploys AI systems and currently has no structured approach to AI-specific security threats. That is most organizations.

The security team that handles the ISMS and runs penetration tests probably does not have a framework for assessing whether the training data pipeline is vulnerable to poisoning. The ML engineering team probably does not have a formal process for evaluating prompt injection resistance. ISO 27090 bridges that gap, not as a compliance requirement, but as a reference for building an AI security practice that does not start from zero.

The companion standard, ISO 27091, covers the privacy side: risks to personal data within AI systems and machine learning models. Together, 27090 and 27091 cover the security-and-privacy surface that AI systems introduce on top of traditional infrastructure.

## The honest limitation

It is guidance, not a mandate. No one will audit you against it. No fine will arrive for ignoring it. In a world where most organizations cannot fully comply with the regulations they already face, another voluntary guidance document competes for attention against everything else.

But the threat taxonomy is sound. The 13 categories are grounded in real attack research. The lifecycle mapping is practical. And the gap it fills, between traditional infosec standards and the AI-specific attack surface, is one that organizations will eventually need to close. Whether they close it now with ISO 27090 or later after an incident is the only real question.
