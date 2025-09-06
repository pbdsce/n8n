# 📧 Team Achievements Email Automation Workflow

This n8n workflow automates the creation and distribution of a monthly team achievements email. It pulls data from a Google Sheet, filters achievements from the last 5 months, generates a professional HTML email using Google Gemini, and includes a full approval process with built-in feedback loops.

-----

## ✨ Features

  * **Google Sheets Trigger**: Automatically monitors a specific Google Sheet for new achievement entries.
  * **Data Processing**: Filters and formats achievements from the last five months into a structured JSON file.
  * **AI-Powered Email Generation**: Utilizes **Google Gemini** to create a polished, HTML-formatted email summary.
  * **Approval Workflow**: Sends the draft email for human approval with options to "Yes," "Improvise," or "Retreat."
  * **Conditional Logic**: Dynamically routes the workflow based on user decisions, enabling a feedback loop for content improvement.
  * **Gmail Integration**: Manages sending emails and waiting for responses using the Gmail API.

-----

## 🏗️ Workflow Structure

1.  **Google Sheets Trigger**: This is the starting point, triggered when a new row is added to the specified Google Sheet.
2.  **Code Node**: Processes the raw data, filtering achievements by date and formatting the JSON for the AI.
3.  **Code1 Node**: Converts the structured JSON into a human-readable string for the AI.
4.  **AI Agent (Google Gemini)**: Generates the HTML email content based on the provided data and instructions.
5.  **Gmail Nodes**: Manages the approval process, including sending the draft email and handling responses.
6.  **If Nodes**: Direct the workflow to either finalize the email, request improvisation, or stop based on the user's choice.
7.  **AI Agent1 (Improvisation Loop)**: If the user requests changes, this node uses feedback to regenerate an improved email draft.
8.  **Final Email Send**: The approved or improvised email is sent to the designated recipient.

-----

## 📋 Prerequisites

  * **n8n Instance**: A running n8n instance (cloud or self-hosted, compatible with n8n-nodes-base 2.x).
  * **Google Account**: An account with access to Google Sheets, Gmail, and the Google Gemini API.
  * **Google Sheet**: A spreadsheet with columns for **`Name`**, **`What_is_the_Achievement`**, and **`What_is_the_date_of_achievement`**.

-----

## 🚀 Installation

### 1\. Clone the Repository

Start by cloning the project from its GitHub repository to your local machine.

```bash
git clone https://github.com/your-username/team-achievements-workflow.git
cd team-achievements-workflow
```

### 2\. Import the Workflow

1.  Copy the JSON content from the **`workflow.json`** file in this repository.
2.  In your n8n instance, go to **Workflows** \> **Import from File/Clipboard**.
3.  Paste the copied JSON and save the workflow.

### 3\. Set Up Credentials

The workflow uses several credentials for Google services. For self-hosted n8n, it is best to manage these securely using environment variables in a **`.env`** file.

  * **Create a `.env` file** in your n8n root directory (e.g., `~/.n8n/.env`).
  * **Add the following variables**, replacing the placeholders with your actual credentials:
    ```ini
    GOOGLE_SHEETS_TRIGGER_OAUTH2_CLIENT_ID=your-google-sheets-client-id
    GOOGLE_SHEETS_TRIGGER_OAUTH2_CLIENT_SECRET=your-google-sheets-client-secret
    GOOGLE_SHEETS_TRIGGER_OAUTH2_REFRESH_TOKEN=your-google-sheets-refresh-token
    GMAIL_OAUTH2_CLIENT_ID=your-gmail-client-id
    GMAIL_OAUTH2_CLIENT_SECRET=your-gmail-client-secret
    GMAIL_OAUTH2_REFRESH_TOKEN=your-gmail-refresh-token
    GOOGLE_GEMINI_API_KEY=your-google-gemini-api-key
    GOOGLE_GEMINI_API_KEY_2=your-second-google-gemini-api-key
    ```
  * **To get your credentials**, go to the Google Cloud Console, enable the necessary APIs (Google Sheets, Gmail, Google Docs, and Google Calendar), and create OAuth 2.0 Client IDs. You will also need to get your Google Gemini API keys from Google AI Studio.
  * **Configure credentials in n8n**: In the n8n UI, go to **Credentials** and add a new credential for each service, using the values from your `.env` file.

### 4\. Configure Google Sheet

  * The workflow is set to use the Google Sheet with ID `12Gl-LE-uxfONjGh9M-bPEH-subo5FEIuDViC2ea93d8`. You can either use this sheet or update the **Google Sheets Trigger** node to point to your own.
  * Ensure your sheet has the correct columns: **`Name`**, **`What_is_the_Achievement`**, and **`What_is_the_date_of_achievement`**.
  * Share the sheet with the Google account you authorized with n8n, granting it edit access.

-----

## ⚙️ Usage

The workflow can be triggered in two ways:

  * **Automatic**: Add a new row to your configured Google Sheet. The workflow will run automatically.
  * **Manual**: Send a POST request to the webhook URL.

**The Workflow Process:**

1.  Filters achievements from the last five months.
2.  Generates a draft email and sends it to `ayushayush9415272949@gmail.com` for approval.
3.  If you reply with **"Yes"**, the final email is sent with the subject **"Monthly Update"**.
4.  If you reply with **"Improvise"**, you'll be prompted for feedback. The AI will regenerate the email, which is then sent for re-approval.
5.  If you reply with **"Retreat"**, the workflow will stop.

-----

## ⚠️ Troubleshooting

  * **Credential Errors**: Double-check your `.env` values and n8n credential configurations.
  * **Sheet Access**: Ensure the sheet is shared with the correct Google account.
  * **AI Failures**: Confirm that the Google Gemini API keys are correct and the input JSON format is valid.
  * **If Node Issues**: Ensure the response values from the email approval match the If conditions.

-----

### 📄 License

This project is licensed under the MIT License. See the `LICENSE` file for details.
