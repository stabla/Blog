---
layout: post
title: "The OAuth Redirect That Phishing Filters Cannot See"
description: "Attackers are abusing OAuth error flows to route victims through legitimate Microsoft infrastructure before dropping them on phishing pages. No fake domains needed."
date: 2026-03-11
tags: [security, OAuth, phishing, web-security, microsoft, 2026]
comments: false
share: true
---

Microsoft published something last week that deserves more attention than it got. Attackers are using OAuth's own error handling mechanism to redirect victims through login.microsoftonline.com, past every email filter and browser warning, straight onto a phishing page or malware drop.

No fake domains. No lookalike URLs. The phishing link points to a real Microsoft endpoint. And it works exactly as the protocol spec says it should.

## How it works

OAuth 2.0 (RFC 6749) specifies that when an authorization request fails for reasons other than a bad redirect URI, the authorization server must redirect the user back to the client's registered redirect_uri with error parameters attached. This is spec-compliant behavior.

The attackers register a malicious OAuth application in a tenant they control. The redirect_uri points to their infrastructure. They craft an authorization URL with deliberately invalid parameters: an impossible scope, a `prompt=none` that forces silent auth failure. Both guarantee an error response. The error response triggers the redirect.

The victim clicks a link to `login.microsoftonline.com/common/oauth2/v2.0/authorize`. Microsoft's authorization server processes it, hits the invalid parameters, and redirects to the attacker's domain. The victim touches real Microsoft infrastructure for a fraction of a second before being bounced to the payload.

One extra detail: the attackers encode the victim's email into the OAuth `state` parameter, which is supposed to be an anti-CSRF token. When the victim lands on the phishing page, the email field is pre-populated. It looks like they were expected.

## Why it matters

Every piece of security advice about phishing tells users to check where the link points. This attack makes that advice useless. The link points to Microsoft. The domain is real. The certificate is valid. The URL path is a legitimate API endpoint.

Email gateways that check URL reputation see a trusted domain. Browser Safe Browsing sees a real Microsoft page. The human who hovers over the link sees login.microsoftonline.com. Every verification step passes.

Microsoft observed two kill chains from this technique. One delivers a ZIP containing an LNK file that triggers PowerShell, drops a legitimate Steam binary (`steam_monitor.exe`), sideloads a malicious DLL, decrypts shellcode in memory, and establishes C2. The other redirects to EvilProxy, an adversary-in-the-middle framework that captures credentials and session cookies in real time, bypassing MFA.

## The deeper problem

There is no CVE for this. The identity provider is doing exactly what the specification tells it to do. The "vulnerability" is that OAuth error handling was designed for a world where all registered applications are trustworthy, and redirect URIs are always benign. That world never existed.

Microsoft's mitigation advice boils down to restricting which OAuth applications users can consent to and auditing registered applications. That is sensible operational hygiene. But it does not fix the protocol-level issue: error redirects through trusted infrastructure will work as long as the spec says they should.

This is what protocol-level trust assumptions look like when they collide with adversarial creativity. The infrastructure is real. The behavior is compliant. The outcome is malicious.
