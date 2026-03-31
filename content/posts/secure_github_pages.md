---
title: "How to Fix 'Enforce HTTPS' on GitHub Pages"
date: 2026-03-31
draft: false
tags: ["problem solving", "https", 'github pages']
summary: "How I solved very annoying problem"
---

For months I couldn't enforce HTTPS on my GitHub Pages site with a custom domain 
purchased through Squarespace. The error message was thoroughly unhelpful: 
*"Not yet available for your site because the certificate has not finished being 
issued. Please allow 24 hours for this process to complete."*

I did everything by the book: set up the four GitHub A records, added a CNAME 
record pointing `www` to `asarmakeeva.github.io`, and watched GitHub's DNS check 
return a green ✅. Yet the TLS certificate kept getting stuck at step 1 of 3 
with a "Certificate Request Error" and never progressed further.

In the age of attention deficit, I couldn't sustain 24 hours of patient waiting — 
so I kept abandoning the problem and coming back to it three months later.

Today I finally fixed it.

## What I tried first (and didn't work)

The most commonly suggested fix is to remove your custom domain from 
Settings → Pages, wait a minute, and re-add it. This forces GitHub to restart 
the certificate provisioning job. It works for a lot of people — but not for me.

## The actual problem: broken DNSSEC

The real diagnostic tool nobody mentions is **[letsdebug.net](https://letsdebug.net)**. 
Enter your domain, select `HTTP-01`, and run the test. In my case it returned 
multiple FATAL errors:
```diff
- DNSLookupFailed: DNS response for www.asarmakeeva.com had fatal DNSSEC issues: 
- validation failure — No DNSKEY record while building chain of trust
```

**DNSSEC** (DNS Security Extensions) is a feature that adds a cryptographic 
layer of trust to DNS records. The problem: Squarespace had it enabled on my 
domain, but the required DNSKEY records were missing or misconfigured. 

This created a paradox: GitHub's own DNS check was passing (it's more lenient), 
but Let's Encrypt — which GitHub uses to issue TLS certificates — performs 
stricter DNSSEC validation and was completely unable to resolve my domain. 
From Let's Encrypt's perspective, my domain simply didn't exist.

## The fix

1. Go to **letsdebug.net**, enter your domain, run the `HTTP-01` test
2. If you see `fatal DNSSEC issues`, head to your DNS provider
3. In Squarespace: **Domains → DNS → DNSSEC → disable DNSSEC**
4. Go back to GitHub Pages, remove and re-add your custom domain to trigger 
   a fresh certificate request
5. Wait ~10 minutes

That's it. Certificate issued, HTTPS enforced, problem solved.

The frustrating part is that GitHub's UI gives you no indication that DNSSEC 
is the culprit — it just says "please be patient" indefinitely. 
Hopefully this saves someone else a few months of on-and-off suffering. enjoy.
