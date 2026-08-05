# BlackHat 2026

Title: This Is Fine: Securing AI While the Room's on Fire
Tracks: AI, ML, & Data Science, Risk, Compliance and Security Management
Length: 20min
Format: Custom reveal.js-ish slides

## Abstract

Vulnerabilities are getting discovered faster than ever, with exploits releasing instantly. Attackers are rolling out automated attacks, and half of your organization is frantically wiring up every new AI tool and agent it can find. It's the fastest-moving attack surface in history.
Here's the security industry, sitting in the burning room with a cup of coffee: Scan, find, report, repeat. We've gotten extraordinary at producing findings and terrible at producing fixes. A dashboard full of risk is not a security program.

This talk is about getting out of the chair. We'll borrow a discipline engineers already trust and apply it to security with agentic AI. You'll see agents add real context to a risk, plan a reviewable fix, and ship it through the tools your team already uses, including securing the AI surface itself. The machine does the work. The human keeps the authority. In 20 minutes, we'll trade "this is fine" for the fire actually being out.

## Approach

- showcase 2-3 recent (up to 5 months ago) Vulnerabilities in AI Agents, AI plugins, or AI components
  - Research 5-7 interesting recent vulns affecting endpoints (Mac/Windows laptops particulary, developers)
  - Research 2-3 interesting recent vulns affecting server infrastructure
- showcase traditional vuln flows (eg: discover > aggregate > prioritize > ticket > report; or find more)
  - outline that it's way too little focus on getting things fixed
- showcase Mondoo's agentic vuln flow (analyze > plan > ship)
  - plan takes prioritized input but does the work traditionally left to security advisors
  - focus on remediations and mitigations to close everything that is found (always have an action to reduce risk)
  - operational considerations for things that are risky and would take time with operations
  - auto-create exceptions and justifications for things that can't move
  - risk assessment of the change itself
  - plan is the easy-mode of "what should I do next?"
  - plan in 3 flavors: thorough, trying to fix as much as you can, comprehensive; low-hanging / easy fixes only, everything low risk to move things; or normal
- think like a platform engineer
  - security at risk of being seen as a blocker, especially to business value when you have to move faster
  - agentic AI has vastly increased the participation in code from other departments
  - use agents to build automations or integrate into existing ones
  - automated flows are easier to audit, test, and revert


# Script


