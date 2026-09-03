# BTC Employers - Agent Skill

Let your AI check, track, verify, and improve your company's record on
[BTC Employers](https://btcemployers.com), the public evidence index of
Bitcoin-friendly employers.

## Fastest way (any AI)

Paste this into Claude, ChatGPT, or any agent that can fetch a URL:

> Fetch https://btcemployers.com/skill.md and follow it: check what the
> BTC Employers index says about my company, verify the evidence still
> holds, and tell me exactly what to publish to improve our record.

## Install as a Claude Code skill

```bash
mkdir -p ~/.claude/skills/btc-employers
curl -o ~/.claude/skills/btc-employers/SKILL.md https://btcemployers.com/skill.md
```

Then ask Claude Code about your company's Bitcoin employer record.

## What it can do

- Find your record in the open dataset (4,900+ companies, 78 countries)
- Read it honestly - `unknown` means not investigated, never "no"
- Verify your evidence link still supports your signals
- Track your record over time and flag changes
- Map exactly what to publish to earn each signal
- Act: claim your record, suggest a company, file a correction

## The one rule

Payment moves visibility. It never moves the truth. That binds us, you,
and your agent - it is the reason the index is worth checking.

The canonical copy of this skill is https://btcemployers.com/skill.md -
agents should fetch that URL rather than caching this repo.
