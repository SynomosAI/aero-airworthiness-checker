# aero-airworthiness-checker · Airworthiness & maintenance qualification domain

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Domain: aero](https://img.shields.io/badge/domain-aero-9cf.svg)](domain.manifest.json)
[![Status: RESERVED](https://img.shields.io/badge/status-RESERVED-orange.svg)](domain.manifest.json)

> **Status: RESERVED (placeholder · open for adoption)** — This repo scaffolds the
> industry-access four-file skeleton per the platform profile and awaits a qualified
> operator to claim it and fill in real rules.

Turns "is this aircraft airworthy, and is the maintenance organisation qualified" into
verifiable attributes: the AI only surfaces prerequisites, it never hands down a release verdict.

**Keywords**: airworthiness · maintenance organisation approval · release-to-service sign-off ·
personnel licence · flight authorisation · CCAR-21 · CCAR-145 · aviation safety · attribute
verification · red lines · gate-map · privacy-by-default · regulatory compliance · MCP checker

## What this domain covers

Airworthiness status, maintenance organisation approval and release-to-service sign-off.
Airworthiness is the bottom line of aviation safety: the certificate, maintenance approval,
release sign-off and personnel licence are tightly coupled. The checker lays out each link's
status — the authority to declare airworthy or release an aircraft belongs only to the
airworthiness authority and authorised personnel.

## Domain identity

| Item | Value |
|---|---|
| Domain ID | `aero` (globally unique, never reused) |
| Domain name | Aerospace · airworthiness & maintenance qualification |
| Profile version | `domain/1.0` |
| Status | `RESERVED` |
| Reserved since | 2026-09-12 |

## Attribute catalogue

| Key | Type | Meaning |
|---|---|---|
| `aero.airworthiness_status` | enum | valid/suspended/revoked/expired/pending/not_required |
| `aero.maintenance_org_approval` | enum | approved/limited/unapproved/not_applicable |
| `aero.released_to_service` | boolean | release-to-service sign-off completed by licensed personnel |
| `aero.personnel_license` | enum | valid/expired/suspended/not_applicable |
| `aero.flight_authorization` | enum | authorized/pending/denied/not_applicable |

## Red lines (machine-readable)

1. Airworthiness status follows the airworthiness certificate and the authority's conclusion; AI must not self-decide.
2. Never output "airworthy", "may release" or "may fly" clearance; release requires an authorised person's signature.
3. Never generate or draft maintenance release sign-off, airworthiness certificates, or flight-authorisation documents.

## The four-file access kit

| File | Role |
|---|---|
| `domain.manifest.json` | Domain declaration: attributes, issuer requirements, TTL, red lines |
| `gate-map.json` | Friction per action: silent / warn / confirm / block / require-owner |
| `privacy.json` | Privacy statement: off by default, minimization, withdrawable, erasable |
| `checker.io.json` | Checker MCP tool I/O contract — shows & verifies only, never judges |

## Quick Start

```bash
git clone https://github.com/SynomosAI/aero-airworthiness-checker.git
cd aero-airworthiness-checker

# Validate the four JSON files with the Python stdlib
python -m json.tool domain.manifest.json > /dev/null
python -m json.tool gate-map.json > /dev/null
python -m json.tool privacy.json > /dev/null
python -m json.tool checker.io.json > /dev/null
```

## Examples

```bash
# List declared attribute keys and their allowed values
python -c "import json;m=json.load(open('domain.manifest.json',encoding='utf-8'));[print(a['key'],'->','/'.join(a.get('values',['bool']))) for a in m['attributes']]"
```

```json
{
  "subject_ref": "mpp:cn:9f2c1a7b",
  "attribute_keys": ["aero.airworthiness_status", "aero.maintenance_org_approval"],
  "evidence": ["tsa:2026-09-12#sha256:3b1f..."],
  "request_id": "req-20260912-0001"
}
```

The checker answers only with `shown / missing / expired / unverifiable` plus the
provenance chain. It never returns "airworthy", "may release", "may fly", or clearance.

## Core principles

The platform is an arena, not a shelf. The checker answers only "is this claim verifiable,
and what prerequisite is missing" — never "is this compliant, should it be done".

## How to adopt

This domain is open to qualified operators: fork the repo, fill in `operator` and
`checker_endpoint`, align attributes and red lines with current rules, run the platform
self-checker (all five criteria must pass), then open a PR with credentials.

## License

Code and configuration are MIT-licensed. Documentation copyright remains with SynomosAI.
See [LICENSE](LICENSE).

© 2026 SynomosAI. All rights reserved.
