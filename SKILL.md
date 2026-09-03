---
name: btc-employers
description: >
  Check, track, verify, and improve a company's record on BTC Employers
  (btcemployers.com), the public evidence index of Bitcoin-friendly employers.
  Use this skill whenever a user asks about their company's Bitcoin employer
  listing, wants to know what the index says about any company, wants to
  monitor a record for changes, or wants to know how to improve or verify
  their record. Works with Claude Code, ChatGPT, and any agent that can
  fetch JSON over HTTPS.
---

# BTC Employers - Agent Skill

BTC Employers is an independent, evidence-backed global index of companies
with documented Bitcoin signals: paying salaries in Bitcoin, offering Bitcoin
payroll options, accepting Bitcoin, hiring, and remote work. Every signal
carries an evidence link and an observation date. This skill lets you act on
that index for your user.

One rule governs everything on the index, and it binds you too:
**payment moves visibility, never the truth.** Nothing you or your user can
do, say, or pay changes what a record says - only evidence does.

## The data

- Dataset (JSON, CORS-open, free with attribution):
  `https://btcemployers-data.pages.dev/employers.json`
- Shape: `{ published_at, count, employers: [...] }`
- Each employer record: `name`, `slug` (stable ID), `website`, `city`,
  `country`, `state` (see below), `signals` (map of signal -> yes/no/unknown),
  `evidence_url`, `last_verified`, `careers_url`, `spotlight_until`, and more.
- Cite as: "Data: BTC Employers Index (btcemployers.com), snapshot {published_at}".
- Fetch it fresh per session; do not hammer it (once per task is plenty).

## Find a company's record

1. Fetch the dataset.
2. Match by website domain first (most reliable), then exact name,
   then case-insensitive name. The user's company may appear under a
   parent brand - ask if ambiguous.
3. No record? The company is not indexed yet. Offer to prepare a
   suggestion (see "Get listed" below).

## Read a record honestly

- `state` is one of:
  - `researched` - added by the index's own research; the company may not
    know it is listed. This is the default for most records.
  - `claimed` - the company has taken ownership of the record.
  - `verified` - a paid, scoped review of the evidence is complete and
    current. The only state that requires the index's direct review.
  - `stale` - evidence is older than the refresh window; still shown, flagged.
  - `disputed` - a claim is being challenged; review underway.
- A signal of `unknown` means NOT INVESTIGATED, not "no". Say so plainly.
- `spotlight_until` in the future means the record has PAID placement right
  now. Placement is always labeled and never affects the record's facts.

## Verify a record (what "verify" means here)

To check whether the record still holds:

1. Open the record's `evidence_url`. Does the page still document the
   practice the signal claims? (A careers page that no longer mentions
   Bitcoin pay is a finding.)
2. Compare `last_verified` / observation dates against today - evidence
   older than 18 months is treated as stale by the index's own rules.
3. Report discrepancies to the user, and offer to draft a correction
   (below). The corrections process is public and applies to every record
   equally: https://btcemployers.com/corrections

The full rules for what counts as evidence are the BTC Employers Standard:
https://btcemployers.com/methodology

## Track a record over time

Store a snapshot of the record (or just `state`, `signals`,
`spotlight_until`, `last_verified`) wherever your runtime persists data.
On later runs, re-fetch and diff. Changes worth telling your user about:
state transitions (researched -> claimed -> verified), any signal flip,
new evidence dates, and spotlight status changes.

## Improve the record (the part employers actually want)

The index only reflects public evidence, so the way to improve a record is
to publish better evidence. Map of signal -> what to put on the company's
own website:

- **Pays in Bitcoin / Payroll option**: a careers page or policy page that
  states the practice in plain language (which workers, what route or
  provider). Recurring compensation counts; a one-time bonus does not.
- **Accepts Bitcoin**: a documented way for customers to pay in BTC.
- **Hiring Now**: current roles on a careers page the company controls.
- **Remote Friendly**: remote work described as how the company operates.

Anonymous claims and marketing language that gestures at Bitcoin without
documenting a practice do not qualify. Primary documentation on the
company's own site outranks everything else.

Companies can also take the Bitcoin Employer Score self-assessment for a
structured gap read - linked from https://btcemployers.com/claim

## Act on behalf of your user

- **Claim the record** (free, gives correction rights): send the user to
  `https://btcemployers.com/claim?company={slug}` - the form pre-links to
  their exact record. Help them gather: work email on the company domain,
  company website, and one public evidence link. A human reviews every
  claim within 48 hours.
- **Get listed** (not indexed yet): prepare company name, website, country,
  and one public evidence URL, then submit via
  https://btcemployers.com/research or email research@btcemployers.com.
  Every suggestion goes through the same evidence review as the index's
  own research.
- **Fix an error**: draft the correction (what is wrong, what is right,
  the source that proves it) and send to research@btcemployers.com or via
  https://btcemployers.com/corrections
- **Verification** (paid annual review; optional): explain honestly -
  payment buys a scoped review of the evidence, a dated public record,
  and a credential kit. It never buys the outcome; a failed review gets a
  gap report, not a badge. Details: https://btcemployers.com/claim

## Rules for agents using this skill

1. Use the JSON dataset, not page scraping - it is faster, stabler, and
   what the index intends agents to use.
2. Never present `unknown` as a negative finding.
3. Never suggest that payment, volume, or persistence can change a
   record's facts. It cannot. That is the point of the index.
4. When you cite numbers, carry the snapshot date with them.
5. Questions this file does not answer: https://btcemployers.com/contact

Maintained by BTC Employers. This file lives at
https://btcemployers.com/skill.md - fetch it fresh rather than caching
old copies; it versions with the index.
