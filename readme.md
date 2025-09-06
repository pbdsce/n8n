#Team Achievements Email Automation Workflow
This repository contains an n8n workflow that automates the creation and distribution of a monthly team achievements email. It pulls data from a Google Sheet, filters achievements from the last 5 months, generates an HTML email using Google Gemini, sends it for approval via Gmail, and handles improvisation feedback before final distribution.
Features

Google Sheets Trigger: Monitors a Google Sheet for new achievement entries.
Data Processing: Filters achievements from the last 5 months and structures them into JSON.
AI-Powered Email Generation: Uses Google Gemini to create a formatted HTML email.
Approval Workflow: Sends the email for approval ("Yes", "Improvise", "Retreat") and handles improvisation feedback.
Gmail Integration: Sends emails and waits for responses using Gmail API.
Conditional Logic: Manages approval and improvisation loops using If nodes.

Workflow Structure

Google Sheets Trigger: Detects new rows in a specified Google Sheet.
Code Node: Processes data, filtering achievements by date and formatting JSON.
Code1 Node: Converts JSON to a readable string for the AI Agent.
AI Agent (Google Gemini): Generates an HTML email with achievements grouped by month.
Gmail Nodes: Send emails for approval, collect responses, and handle improvisations.
If Nodes: Route the workflow based on approval responses.
Webhook: Optional manual trigger endpoint.

Prerequisites

n8n: A running n8n instance (cloud or self-hosted, version compatible with n8n-nodes-base 2.x).
Google Account: With access to Google Sheets, Gmail, and optionally Google Docs/Calendar.
Google Sheet: Containing columns Name, What_is_the_Achievement, and What_is_the_date_of_achievement.
Credentials: OAuth2 tokens for Google services and Google Gemini API keys.

Installation
1. Clone the Repository
git clone https://github.com/your-username/team-achievements-workflow.git
cd team-achievements-workflow

2. Import the Workflow

Copy the JSON from workflow.json in the repository.
In n8n, navigate to Workflows > Import from File/Clipboard.
Paste the JSON and save the workflow as "Team Achievements Email".

3. Set Up Environment Variables
To securely manage credentials, use a .env file.

Create a .env File:

In the n8n root directory (e.g., ~/.n8n/), create a .env file:
touch ~/.n8n/.env


Add the following environment variables with your Google credentials:
GOOGLE_SHEETS_TRIGGER_OAUTH2_CLIENT_ID=your-google-sheets-client-id
GOOGLE_SHEETS_TRIGGER_OAUTH2_CLIENT_SECRET=your-google-sheets-client-secret
GOOGLE_SHEETS_TRIGGER_OAUTH2_REFRESH_TOKEN=your-google-sheets-refresh-token
GOOGLE_DOCS_OAUTH2_CLIENT_ID=your-google-docs-client-id
GOOGLE_DOCS_OAUTH2_CLIENT_SECRET=your-google-docs-client-secret
GOOGLE_DOCS_OAUTH2_REFRESH_TOKEN=your-google-docs-refresh-token
GOOGLE_CALENDAR_OAUTH2_CLIENT_ID=your-google-calendar-client-id
GOOGLE_CALENDAR_OAUTH2_CLIENT_SECRET=your-google-calendar-client-secret
GOOGLE_CALENDAR_OAUTH2_REFRESH_TOKEN=your-google-calendar-refresh-token
GMAIL_OAUTH2_CLIENT_ID=your-gmail-client-id
GMAIL_OAUTH2_CLIENT_SECRET=your-gmail-client-secret
GMAIL_OAUTH2_REFRESH_TOKEN=your-gmail-refresh-token
GOOGLE_GEMINI_API_KEY=your-google-gemini-api-key
GOOGLE_GEMINI_API_KEY_2=your-second-google-gemini-api-key




Obtain Google OAuth2 Credentials:

Go to Google Cloud Console.
Create a project or select an existing one.
Enable APIs: Google Sheets, Gmail, Google Docs (optional), Google Calendar (optional).
Create OAuth 2.0 Client IDs (Credentials > Create Credentials > OAuth 2.0 Client IDs).
Set up as a Web application or Desktop app, noting Client ID, Client Secret, and Redirect URI.
Generate a refresh token using an OAuth2 flow (e.g., via Postman or n8n’s OAuth2 setup).
Ensure Gmail scopes include https://www.googleapis.com/auth/gmail.send and https://www.googleapis.com/auth/gmail.readonly.


Obtain Google Gemini API Keys:

Visit Google AI Studio or Google Cloud Console.
Generate two API keys for Google Gemini (PaLM) for the two AI Agent nodes.
Add them to the .env file as GOOGLE_GEMINI_API_KEY and GOOGLE_GEMINI_API_KEY_2.


Load Environment Variables:

For self-hosted n8n, place the .env file in ~/.n8n/ or your configured directory.
Restart n8n to load the variables:n8n restart


For Docker setups, add environment variables to your docker-compose.yml:services:
  n8n:
    image: n8nio/n8n
    environment:
      - GOOGLE_SHEETS_TRIGGER_OAUTH2_CLIENT_ID=${GOOGLE_SHEETS_TRIGGER_OAUTH2_CLIENT_ID}
      - GOOGLE_SHEETS_TRIGGER_OAUTH2_CLIENT_SECRET=${GOOGLE_SHEETS_TRIGGER_OAUTH2_CLIENT_SECRET}
      - GOOGLE_SHEETS_TRIGGER_OAUTH2_REFRESH_TOKEN=${GOOGLE_SHEETS_TRIGGER_OAUTH2_REFRESH_TOKEN}
      # ... other variables


For cloud-hosted n8n, add variables via the hosting provider’s environment settings.


Configure Credentials in n8n:

In n8n, go to Credentials > Add Credential.
Add credentials for:
Google Sheets Trigger OAuth2 (Google Sheets Trigger account)
Google Docs OAuth2 (Google Docs account, optional)
Google Calendar OAuth2 (Google Calendar account, optional)
Gmail OAuth2 (Gmail account)
Google Gemini API (Google Gemini(PaLM) Api account and Google Gemini(PaLM) Api account 2)


Use values from the .env file or enter them manually.



4. Configure Google Sheet

Use the Google Sheet with ID 12Gl-LE-uxfONjGh9M-bPEH-subo5FEIuDViC2ea93d8 (Sheet1).
Ensure columns: Name, What_is_the_Achievement, What_is_the_date_of_achievement (date formats: YYYY, YYYY-MM, or YYYY-MM-DD).
Share the sheet with the Google service account or OAuth2 user with edit access.

Usage

Trigger the Workflow:
Automatic: Add a new row to the Google Sheet to trigger the workflow.
Manual: Send a POST request to the webhook URL (f60944a9-8567-4d04-b452-2c9bb58f6f31).


Workflow Process:
Filters achievements from the last 5 months (based on 2025-09-05 in testing; update to new Date() for production).
Generates an HTML email with achievements grouped by month.
Sends the email to ayushayush9415272949@gmail.com for approval with options: "Yes", "Improvise", "Retreat".
If "Improvise", collects feedback, regenerates the email, and re-sends for approval.
If "Yes", sends the final email with subject "Monthly Update".
If "Retreat", stops the workflow.


Output Example:Subject: Team Achievements – April 2025 to August 2025

Hi Team,

Over the past five months, we've achieved some remarkable milestones together! Below is a summary of our accomplishments:

**August 2025**
- **John Doe**: Completed Project X (Date: 2025-08-15)

**July 2025**
- **Jane Smith**: Won Team Award (Date: 2025-07-10)

**June 2025**
- No specific achievements recorded for this month.

**May 2025**
- No specific achievements recorded for this month.

**April 2025**
- No specific achievements recorded for this month.

Let's keep pushing the boundaries of what's possible! Your hard work and dedication are truly inspiring.

The Dark Knight



Testing

Manual Execution: Run the workflow in n8n’s UI to test.
Google Sheet Trigger: Add a test row to the sheet.
Webhook Trigger: Use a tool like curl or Postman to send a request to the webhook.
Verify Emails: Check ayushayush9415272949@gmail.com for approval and final emails.
Debug: Review console logs in the Code node and n8n’s Execution Log.

Troubleshooting

Credential Errors: Verify .env values and n8n credential configurations.
Sheet Access: Ensure the sheet is shared with the OAuth2 account.
Email Issues: Confirm Gmail API scopes and sending quotas.
AI Agent Failures: Check Google Gemini API keys and input JSON format.
If Node Issues: Ensure response values match If conditions ("Yes", "Improvise", "Retreat").

Notes

The Code node uses a hardcoded date (2025-09-05) for testing. Replace with new Date() for real-time use.
Google Docs and Google Calendar nodes are included but disabled; enable for additional functionality.
Secure the .env file and exclude it from version control (add to .gitignore).
Monitor Google API rate limits and Gmail sending quotas in production.

Contributing

Fork the repository and submit pull requests for improvements.
Report issues or suggest features via GitHub Issues.

License
MIT License. See LICENSE for details.
