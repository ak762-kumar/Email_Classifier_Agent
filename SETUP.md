# Setup Guide

This guide explains how to set up the Email Classifier Agent on a local machine.

## 1. Install Python

Make sure Python 3.10+ is installed and available in your PATH.

Check:

```bash
python --version
```

## 2. Create and activate a virtual environment

From the project folder:

```bash
python -m venv .venv
```

On Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

## 3. Install dependencies

```bash
pip install anthropic google-api-python-client google-auth-oauthlib
```

## 4. Set the Anthropic API key

Create a `.env` file or export the environment variable before running the script:

PowerShell:

```powershell
$env:ANTHROPIC_API_KEY = "your_api_key_here"
```

Or in a `.env` file:

```env
ANTHROPIC_API_KEY=your_api_key_here
```

## 5. Configure Google OAuth

You need a Google Cloud project with Gmail API enabled and an OAuth client configuration file named `credentials.json`.

Steps:

1. Go to Google Cloud Console
2. Create or select a project
3. Enable the Gmail API
4. Create OAuth 2.0 Client ID
5. Download the JSON and save it as:
   `credentials.json`

Place it in the project folder.

## 6. First-time Gmail login

Run the script once manually to let Google complete the OAuth flow and create `token.json`:

```bash
python email_agent.py list-rules
```

This should open a browser and prompt you to sign into your Google account.

After the first successful login, a `token.json` file will be created automatically.

## 7. Configure rules

You can add your own rules interactively:

```bash
python email_agent.py add-rule "Emails about job opportunities, interview invites, and recruiter outreach"
```

List current rules:

```bash
python email_agent.py list-rules
```

## 8. Preview before changing mail

Preview a sample of emails without moving anything:

```bash
python email_agent.py preview --n 20
```

## 9. Backfill existing mail

This scans historical Gmail mail and classifies it:

```bash
python email_agent.py backfill --yes
```

To process only newer mail:

```bash
python email_agent.py backfill --months 6 --yes
```

## 10. Incremental mode

Use hourly or cron-based processing for new mail:

```bash
python email_agent.py incremental
```

To move sorted mail out of the inbox:

```bash
python email_agent.py mode move
```

To leave mail in the inbox and only label it:

```bash
python email_agent.py mode label
```

## 11. Undo previous actions

If you want to revert the agent’s work:

```bash
python email_agent.py undo
```

Or only for a specific folder:

```bash
python email_agent.py undo --folder "Transactions"
```

## 12. Cron example

Example hourly cron command:

```bash
0 * * * * cd /path/to/project && set -a && . ./.env && set +a && venv/bin/python email_agent.py incremental >> agent.log 2>&1
```

## Troubleshooting

### Import errors

If you see a missing library error, reinstall dependencies:

```bash
pip install -r requirements.txt
```

If you do not have a `requirements.txt`, use the original install command again.

### Google authentication fails

Delete `token.json` and run the script again to reauthorize:

```bash
Remove-Item token.json
```

### The agent won’t start

Check:

- `credentials.json` exists
- `ANTHROPIC_API_KEY` is set
- the Python environment is activated
- the Gmail API is enabled in Google Cloud

## Recommended first run

1. Activate the environment
2. Install dependencies
3. Add `credentials.json`
4. Run `python email_agent.py list-rules`
5. Add a few rules
6. Run `python email_agent.py preview --n 10`
7. Run `python email_agent.py backfill --yes`

