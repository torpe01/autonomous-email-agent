# Autonomous AI Email & Calendar Agent 🤖📅

An intelligent, autonomous personal assistant built on n8n that manages your inbox, schedules meetings, and answers questions using Google Gemini.

This workflow goes beyond simple automation—it acts as an agent that can "read" your history to answer questions, check your real-time availability to book meetings, and filter noise by logging only high-priority items.

**The Best part is you can customise your System Massage According to your need.**

## 🚀 Key Features
 🧠 Intelligent Triage: Analyzes every incoming email and assigns a Priority Score (1-10) based on context, sender, and urgency.

 🗓️ Autonomous Scheduling:

  - Availability Check: Uses a specialized tool to "read" your Google Calendar and detect conflicts before making promises.

  - Booking: Automatically books slots for agreed times.

  - Negotiation: If you are busy, it drafts a polite negotiation email asking for a different time.

 🔍 Context-Aware Replies: If a user asks a question (e.g., "What was the budget for Project Alpha?"), the Agent searches your Gmail History, finds the answer, and drafts a fact-based reply.

 📊 Smart Logging: Automatically logs emails to Google Sheets with structured data (Summary, Priority, Draft Status), but only if the Priority is 5 or higher.

 📝 Structured Output: Uses a strict JSON schema to ensure every email is processed with consistent data fields.

## 🛠️ Tech Stack

- Platform: n8n (Self-Hosted or Cloud)

- AI Model: Google Gemini (via LangChain)

Integrations:

- Gmail: (Trigger, Read Messages, Create Draft)

- Google Calendar: (Search Events, Create Event)

- Google Sheets: (Logging & Reporting)

## 📋 Prerequisites

- Before importing the workflow, ensure you have:

  - n8n Installed: You need an active n8n instance.

- Google Cloud Console Project:

  - Enable Gmail API, Google Calendar API, and Google Sheets API.

- Create OAuth 2.0 Credentials (Client ID & Client Secret) with the appropriate scopes.

- Google Gemini API Key: Get a free or paid API key from Google AI Studio.

- Google Sheet: Create a new Google Sheet with the following headers in Row 1:

  1. Date

  2. Priority

  3. Subject

  4. Original Sender

  5. draft_generated

  6. Summary

## ⚙️ Setup Guide

1. Google Cloud OAuth2 Setup (Step-by-Step)

Since this agent interacts with sensitive data, you must create a custom Google Cloud App.

- Create Project: Go to Google Cloud Console and create a new project.

- Enable APIs: Go to APIs & Services > Library and search for/enable these three APIs:

  - Gmail API

  - Google Calendar API

  - Google Sheets API

Configure Consent Screen:

- Go to APIs & Services > OAuth consent screen.

- Select External (unless you are in a Workspace organization).

- Fill in the App Name and Support Email.

- Scopes: Click "Add or Remove Scopes" and add:

  - .../auth/gmail.modify

  - .../auth/calendar

  - .../auth/spreadsheets

- Test Users: Add your own email address here (Critical for testing).

Create Credentials:

- Go to Credentials > Create Credentials > OAuth client ID.

- Application Type: Web application.

- Authorized redirect URIs: Paste the callback URL from your n8n credential window. It typically looks like:
  - https://[your-n8n-domain]/rest/oauth2-credential/callback

- Click Create and copy your Client ID and Client Secret.

2. Import the Workflow

- Download the clean_workflow.json file from this repository.

- Open your n8n dashboard.

- Click "Import from File" and select the JSON file.

3. Connect n8n Credentials

- Go to the n8n Credentials section and create new credentials for:

- Gmail / Calendar / Sheets: Select "OAuth2" and paste the Client ID and Client Secret you generated in Step 1.

- Google Gemini (PaLM) API: Paste your API Key from Google AI Studio.

4. Critical Configuration (Do Not Skip)

- The workflow contains placeholders that you must replace with your actual data.

A. Connect the Google Sheet 📊

The Google Sheets node currently has a placeholder ID (SHEET_ID_PLACEHOLDER). You must link it to your actual file:

- Open the node named "Append row in sheet".

- Document: Click the dropdown, select "From List", and choose your Google Sheet file.

- Sheet: Click the dropdown, select "From List", and choose the specific tab (e.g., Sheet1).

B. Update Calendar Email 📅

- The Calendar tools need to know whose calendar to manage.

- Open the "Create an event in Google Calendar" node.

- Replace USER_EMAIL_PLACEHOLDER with your actual Gmail address.

- Open the "Get many events in Google Calendar" node.

- Replace USER_EMAIL_PLACEHOLDER with your actual Gmail address.

5. Customize the "Brain" (System Message)

Open the AI Agent node. In the "System Message" section, you will see the Priority Rules. Customize these to match your needs:

- High Priority: Add your manager's name or key project keywords.

- Low Priority: Add newsletters or specific domains you want to ignore.

## 🧪 How to Test

Activate the workflow (Toggle "Active" in the top right).

- Send a Test Email: Send an email to yourself from a secondary account asking: "Are you free for a quick call this Friday at 3 PM?"

- Watch it Work:

  - The Agent will check your calendar using the Get many events tool.

  - If free, it will book the meeting using the Create an event tool.

  - It will create a draft reply in your "Drafts" folder.

  - It will log the activity to your Google Sheet (since Priority is > 5).

📄 License

MIT License
