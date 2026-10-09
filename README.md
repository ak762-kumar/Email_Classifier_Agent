# Email Classifier Agent

This project is a personal Gmail email triage agent that uses Anthropic Claude to automatically classify incoming mail into user-defined folders.

## What it does

- Reads Gmail messages and inspects subject + body text
- Uses a configured set of filing rules
- Sends each email to Claude for classification
- Routes emails into folders such as:
  - Potential Opportunity
  - Transactions
  - Socials
- Optionally moves sorted mail out of the inbox instead of only labeling it
- Keeps an audit log so actions can be reversed with undo
- Supports both full backfill and incremental hourly processing

## Main features

- `add-rule`: add a new filing rule in plain English
- `list-rules`: show all active rules
- `preview`: test classification on a sample without changing mail
- `backfill`: classify historical Gmail mail in batches
- `incremental`: process new inbox mail automatically
- `undo`: revert the agent’s prior actions using the audit log
- `mode`: choose between `move` and `label` behavior

## Project structure

- `email_agent.py` — main agent logic
- `rules.json` — user-defined classification rules
- `state.json` — resumable backfill state
- `audit.jsonl` — log of applied actions for undo support
- `token.json` — Gmail OAuth token cache
- `credentials.json` — Google OAuth client config
- `agent.lock` — lock file to prevent overlapping runs

## Notes

This is designed as a personal automation script for a single Gmail account. It is not a full production SaaS system, but it is useful for personal mailbox automation.

---

## Security and safety notes

- The agent treats email content as untrusted input.
- It never follows instructions embedded in the email body.
- Model output is validated before acting on it.
- The script uses Gmail labels and the Gmail modify scope, not broad account access.

