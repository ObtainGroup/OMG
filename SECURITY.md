# Security Policy

Obtain Message Gateway (OMG) moves business documents between Microsoft
Dynamics 365 and external systems. We take reports about its security
seriously and would rather hear about a problem early than read about it
later. Thank you for taking the time to report one.

## Reporting a vulnerability

**Please do not open a public GitHub issue for a suspected vulnerability, and
do not describe it in a pull request, discussion or social media post before
we have had a chance to respond.**

Report it privately, in either of these ways:

- **GitHub:** use the *Report a vulnerability* button under the repository's
  **Security** tab (GitHub private vulnerability reporting). This is the
  preferred route — it keeps the report, our replies and the eventual advisory
  in one place.
- **Email:** security@obtain.dk

Reports may be written in English or Danish.

If you would like to encrypt your report, ask us for a key at
security@obtain.dk before sending details, or open the report on GitHub
instead.

### What to include

The more of this you can give us, the faster we can confirm and fix:

- The OMG version (model version from the `OMG` descriptor) and the Dynamics
  365 platform / application version you tested on.
- Whether you tested an unmodified release or a modified or forked model.
- What an attacker can achieve — read data, alter or replay a message, escalate
  privilege, bypass an approval, cause message loss.
- Steps to reproduce, ideally with the smallest configuration that shows it.
- Any relevant X++ objects, endpoints, security roles, duties or privileges
  involved.
- Whether you believe the issue is already being exploited.

Please send us the details even if you are not certain the finding is
exploitable. We would rather triage a false positive than miss a real issue.

## What happens next

These are targets we work to, not contractual commitments. Nothing in this
file creates an obligation on Obtain ApS; see [SUPPORT.md](SUPPORT.md) and
[LICENSE.md](LICENSE.md).

| Stage | Target |
| --- | --- |
| Acknowledge your report | 5 working days |
| Initial assessment and severity | 10 working days |
| Status update while work continues | At least every 14 days |

We will tell you whether we accept the finding, what severity we assigned and
why, and — once we have one — our intended fix and release timing. If we
decide not to act on a report, we will say so and explain the reasoning
rather than going quiet.

Handling of a security report does **not** depend on whether you hold an OMG
subscription. Anyone may report, and we will treat the report the same way.

## Coordinated disclosure

We ask for **90 days** from your report before public disclosure, or until a
fix is released, whichever comes first. If a fix is going to take longer than
that, we will tell you why and agree an extension with you rather than let the
deadline pass silently.

When we publish a fix we will issue a GitHub Security Advisory and credit you
by the name or handle you choose, unless you prefer to stay anonymous. We do
not currently run a paid bug bounty programme.

## Supported versions

Security fixes are issued for the **latest published release only**. Older
releases receive no security fixes, including releases that have passed their
Change Date and converted to the Apache License 2.0 under
[LICENSE.md](LICENSE.md). If you run a fork or a modified OMG model, you are
responsible for applying our fixes to it.

## Scope

**In scope** — the OMG model as published in this repository: X++ code,
metadata, security roles, duties and privileges, data entities and services,
integration endpoints, and the build and release files here.

**Out of scope:**

- Vulnerabilities in Microsoft Dynamics 365, the Finance and Operations
  platform, Azure or any other Microsoft product. Report those to the
  Microsoft Security Response Center (https://msrc.microsoft.com).
- A customer's own configuration, security-role assignment, network setup or
  credential handling.
- Extensions, customisations or forks built on top of OMG by third parties.
- Findings from automated scanners with no demonstrated impact on OMG.
- Missing hardening that has no exploitable consequence, absent a realistic
  attack path.
- Social engineering, physical attacks, and denial of service through sheer
  volume.

## Testing safely

If you want to probe OMG, please do it **in your own Dynamics 365 development
or sandbox environment** — which the licence permits free of charge.

Do not test against Obtain ApS systems, against another organisation's
tenant, or against any production environment. Do not access, modify, exfiltrate
or retain data belonging to anyone else; if you encounter someone else's data,
stop and tell us. Note that security testing against Microsoft-hosted Dynamics
365 environments is also governed by Microsoft's own rules for penetration
testing of its cloud services — those apply to you regardless of anything we
say here.

If you follow this policy, act in good faith, and stay within your own
environments, we will not pursue or support legal action against you over your
research, and we will say so if a third party asks.

## Regulatory reporting

Where a vulnerability in OMG is subject to a statutory reporting obligation —
including reporting of actively exploited vulnerabilities under the EU Cyber
Resilience Act — Obtain ApS will make those reports as required. If you have
reason to believe a vulnerability you are reporting is already being
exploited, please say so prominently in your report so we can prioritise it.

## Contact

security@obtain.dk
