


🏠 Real Estate Voice AI Automation
An AI-powered real estate lead management automation built with n8n, Vapi, Google Gemini, Google Sheets, Gmail, and Google Calendar.

The workflow converts voice conversations into structured property leads, stores them automatically, notifies the sales team, confirms the inquiry with the customer, and can schedule site visits.

✨ Features
🎙️ Voice-based lead intake via Vapi/Webhook

🤖 AI-powered lead understanding with Google Gemini

📋 Automatic lead data extraction

📊 Save leads to Google Sheets

📧 Admin email notifications

📩 Customer confirmation email

📅 Automatic site-visit scheduling with Google Calendar

🔄 End-to-end automated lead workflow

🔄 Workflow 
Vapi / Voice Assistant
        ↓
      Webhook
        ↓
JavaScript Lead Extraction
        ↓
     AI Agent
     ↙     ↘
 Gemini     Calendar
        ↓
  Google Sheets
        ↓
   Admin Email
        ↓
 Customer Email
 
🧩 Tech Stack
Technology	Purpose <br/>
n8n	Workflow automation
Vapi	Voice AI / call handling
Google Gemini	AI processing
JavaScript	Lead data extraction
Google Sheets	Lead storage
Gmail	Email notifications
Google Calendar	Site-visit scheduling
📌 Lead Information
The workflow can process:

Name

Phone

Email

Property type

Bedrooms / bathrooms

Budget

Location

Site-visit preference

Conversation transcript

AI-generated intent/summary

🚀 Setup
1. Import Workflow
Import:

Real-Estate-Voice-Assistant-GitHub-Safe.json
into your n8n instance.

2. Configure Credentials
Reconnect your own credentials for:

Google Gemini

Google Sheets

Gmail

Google Calendar

Credentials are intentionally not included in the GitHub-safe workflow.

3. Configure Google Sheets
Create a sheet with columns similar to:

Name | Phone | Property | Budget | Location |
Visit Site | AI Summary | Date & Time | E-mail
Then select your spreadsheet in the Google Sheets node.

4. Configure Webhook
Set the n8n Webhook URL as the server/webhook endpoint in your Vapi assistant.

The webhook uses:

POST
5. Test & Activate
Test the workflow with a sample call, verify:

Lead extraction

Gemini response

Google Sheets entry

Calendar event

Admin notification

Customer email

Then activate the workflow.

🔐 Security
Never commit:

API keys
Access tokens
OAuth secrets
Passwords
.env files
Customer PII
Production credentials
Always use your own credentials and review workflow-specific IDs, emails, webhook URLs, and test data before making the repository public.

📁 Repository Structure
real-estate-voice-ai-automation/
├── README.md
├── Real-Estate-Voice-Assistant-GitHub-Safe.json
├── .gitignore
└── LICENSE
🎯 Use Cases
Real estate agencies

Property developers

Lead-generation systems

AI voice assistants

Automated sales workflows

n8n automation portfolios

👨‍💻 Author
Md Nahid

AI Automation & Full-Stack Developer

⭐ If you find this project useful, consider starring the repository.
