Real Estate Voice AI Automation

An AI-powered real estate lead qualification and follow-up workflow built with n8n, Vapi, Google Gemini, Google Sheets, Gmail, and Google Calendar.

The workflow receives a voice-assistant webhook payload, extracts important customer and property information, uses an AI Agent to summarize the lead and handle site-visit scheduling, stores the lead in Google Sheets, sends an internal notification to the real estate team, and sends a confirmation email to the customer.

Repository: real-estate-voice-ai-automation

📌 Project Overview

This project automates the first stage of a real estate sales process.

Instead of manually collecting information from every phone conversation, the workflow turns a voice conversation into a structured lead record.

The workflow collects

Customer name

Phone number

Email address

Preferred location

Property type

Number of bedrooms

Number of bathrooms

Budget

Preferred site-visit date/time

Conversation transcript

Lead intent / summary

After collecting the information, the workflow can

Receive the voice assistant webhook request.

Parse and normalize the transcript.

Extract structured lead information with JavaScript.

Send the lead data to an AI Agent.

Use Google Gemini as the AI language model.

Schedule a Google Calendar event when a site visit is requested.

Save the lead in Google Sheets.

Notify the admin/team by email.

Send a confirmation email to the customer.

🏗️ Architecture

Voice Assistant / Vapi
        │
        ▼
   ┌───────────┐
   │  Webhook  │
   └─────┬─────┘
         │
         ▼
┌──────────────────────┐
│ Code in JavaScript   │
│ Lead Data Extraction  │
└──────────┬───────────┘
           │
           ▼
    ┌─────────────┐
    │  AI Agent   │◄──────────────┐
    └──────┬──────┘               │
           │                      │
           ▼                      │
┌─────────────────────┐    ┌─────────────────────┐
│ Google Sheets       │    │ Google Gemini       │
│ Lead Storage        │    │ Chat Model          │
└──────────┬──────────┘    └─────────────────────┘
           │
           ▼
    ┌───────────────┐
    │ Notify Admin  │
    └───────┬───────┘
            │
            ▼
    ┌───────────────┐
    │ Notify User   │
    └───────────────┘

Google Calendar
      │
      └──────► AI Agent Tool

✨ Main Features

1. Voice Lead Intake

The workflow starts with an n8n Webhook node using an HTTP POST request.

A voice AI platform such as Vapi can send the conversation data to this webhook after or during a call.

The webhook payload may contain:

Customer information

Call metadata

Transcript messages

Assistant information

Conversation timestamps

2. Intelligent Lead Extraction

The Code in JavaScript node converts the incoming voice transcript into structured JSON.

The extraction logic handles fields such as:

name
phone
email
property
budget
location
visit_sites
intent
transcript
date_time

Example output:

{
  "date_time": "10/1/2026, 11:00:00 AM",
  "name": "John",
  "phone": "01712345678",
  "email": "john@example.com",
  "property": "3 Bed 2 Bath Apartment",
  "budget": "1.2 Crore BDT",
  "location": "Uttara",
  "visit_sites": "tomorrow at 11 AM",
  "intent": "Looking for 3 Bed 2 Bath Apartment in Uttara with budget 1.2 Crore BDT",
  "transcript": "AI: ...\nUser: ..."
}

The exact extraction behavior is defined by the JavaScript inside the workflow. If your voice assistant uses different wording, locations, or data formats, you may need to customize the extraction rules.

🤖 AI Agent

The AI Agent receives the structured lead information and transcript.

Its main responsibilities are:

Understand the customer's intent.

Review the property requirements.

Determine whether a site visit was requested.

Use the Google Calendar tool when a site visit should be scheduled.

Produce a concise summary of the lead.

Indicate the immediate next step.

The current workflow prompt is designed around real estate lead management and site-visit scheduling.

🧠 Google Gemini

The workflow uses Google Gemini Chat Model as the AI language model connected to the AI Agent.

Connection:

Google Gemini Chat Model
          │
          ▼
       AI Agent

You must connect your own Gemini credential after importing the workflow.

Required

Google AI / Gemini API access

Gemini API credential configured in n8n

The Gemini Chat Model node connected to the AI Agent

📅 Google Calendar Integration

The workflow contains a Create an event in Google Calendar node connected to the AI Agent as an AI tool.

AI Agent
   │
   └── Google Calendar Tool

When a customer requests a site visit, the AI Agent can use this tool to create a calendar event.

The event can include customer information such as:

Customer name

Phone number

Email

Property requirement

Location

Budget

Site visit information

Important

The imported GitHub-safe workflow does not include the original Google Calendar credential.

After importing the workflow:

Open the Google Calendar node.

Create/select your own Google Calendar OAuth credential.

Select the calendar you want to use.

Test event creation.

Save the workflow.

📊 Google Sheets Lead Management

The Append row in sheet node stores each qualified lead in Google Sheets.

The workflow maps the following columns:

Column

Description

Name

Customer name

Property

Property type and requirements

Phone

Customer phone

Budget

Customer budget

Location

Preferred location

AI Summary

AI-generated lead summary

Visit Site

Requested site-visit date/time

Date & Time

Lead/call timestamp

E-mail

Customer email

Recommended Google Sheet structure

Create a spreadsheet with a sheet such as:

Sheet1

Use headers similar to:

Name
Phone
Property
Budget
Location
Visit Site
AI Summary
Date & Time
E-mail

Then connect the Google Sheets node to your own spreadsheet.

📧 Admin Notification

After the lead is saved to Google Sheets, the workflow sends an email notification to the internal team.

The admin email contains information such as:

Date & Time

Customer name

Phone number

Email

Property

Budget

Location

Site-visit preference

Customer intent

Conversation transcript

This allows the sales team to receive a lead notification without manually checking the spreadsheet.

📩 Customer Confirmation Email

The Notify User node sends a confirmation email to the customer.

The email confirms that the customer's property inquiry has been received.

It can include:

Customer name

Property type

Location

Budget

Site-visit preference

Phone number

Real estate team follow-up message

Before production use, configure this node to send to the customer's email field instead of a fixed testing address.

🔄 Complete Workflow

The main execution path is:

1. Webhook
      ↓
2. Code in JavaScript
      ↓
3. AI Agent
      ↓
4. Append row in Google Sheets
      ↓
5. Notify Admin
      ↓
6. Notify User

Additional AI connections:

Google Gemini Chat Model
        ↓
     AI Agent

and:

Google Calendar
        ↓
   AI Agent Tool

🧩 n8n Nodes

The workflow contains these 8 nodes:

#

Node

Purpose

1

Webhook

Receives voice-assistant data

2

Code in JavaScript

Extracts structured lead information

3

AI Agent

Understands lead intent and manages the next step

4

Google Gemini Chat Model

Provides the AI language model

5

Append row in sheet

Stores the lead

6

Notify Admin

Sends internal lead notification

7

Notify User

Sends customer confirmation

8

Create an event in Google Calendar

Schedules site visits

🛠️ Requirements

Before setting up the project, install or prepare:

Required

n8n

A Vapi voice assistant or another webhook-compatible voice platform

Google Gemini API access

Google account

Google Sheets

Google Calendar

Gmail / Google OAuth access

Recommended

n8n Cloud or a self-hosted n8n instance

A public HTTPS webhook URL

A dedicated Google account for automation

A dedicated Google Sheet for leads

A dedicated calendar for site visits

🚀 Installation & Setup

Step 1 — Download the Workflow

Download the GitHub-safe workflow JSON from this repository.

Example filename:

Real-Estate-Voice-Assistant-GitHub-Safe.json

Step 2 — Open n8n

Open your n8n instance.

For example:

https://your-n8n-domain.com

or your local n8n installation.

Step 3 — Import the Workflow

In n8n:

Open Workflows.

Create/open a workflow.

Select Import from File.

Choose:

Real-Estate-Voice-Assistant-GitHub-Safe.json

Import the workflow.

You should see the complete workflow with the 8 nodes.

🔐 Step 4 — Configure Credentials

The GitHub-safe workflow intentionally removes stored credential references.

You must reconnect your own credentials.

Google Gemini

Open:

Google Gemini Chat Model

Configure your Gemini credential.

Google Sheets

Open:

Append row in sheet

Create/select your Google Sheets OAuth credential.

Then select your own spreadsheet and worksheet.

Gmail

Open:

Notify Admin

and:

Notify User

Configure your own Gmail OAuth credential.

Google Calendar

Open:

Create an event in Google Calendar

Configure your own Google Calendar OAuth credential and select your calendar.

🌐 Step 5 — Configure the Webhook

Open:

Webhook

The workflow expects:

HTTP Method: POST

After activating the workflow, n8n will provide a production webhook URL.

Use that URL in your voice assistant/Vapi configuration.

Example

POST https://your-n8n-domain.com/webhook/your-webhook-path

Do not publish your private production webhook URL in public documentation if you do not want external users to trigger your workflow.

🎙️ Step 6 — Configure Vapi

Your voice assistant should send the call/conversation data to the n8n webhook.

The general flow is:

Customer
   ↓
Vapi Voice Assistant
   ↓
Conversation / Transcript
   ↓
n8n Webhook
   ↓
Lead Automation

Make sure the Vapi payload contains enough transcript information for the JavaScript node to process it.

The current workflow looks for transcript data from structures such as:

message.artifact.messages
message.transcript
body.transcript

If your provider sends a different JSON structure, update the Code node accordingly.

🧪 Step 7 — Test the Webhook

Before activating production automation, test the webhook.

A test payload should contain conversation data similar to:

{
  "message": {
    "transcript": [
      {
        "role": "user",
        "message": "My name is John."
      },
      {
        "role": "assistant",
        "message": "What property are you looking for?"
      },
      {
        "role": "user",
        "message": "I need a 3 bedroom apartment in Uttara."
      }
    ]
  }
}

Your exact payload depends on the voice platform.

🧪 Step 8 — Test the JavaScript Node

Run:

Webhook
   ↓
Code in JavaScript

Verify that the Code node produces fields such as:

name
phone
email
property
budget
location
visit_sites
intent
transcript
date_time

If values are N/A, inspect the incoming webhook payload and update the extraction logic.

🤖 Step 9 — Test the AI Agent

Run the AI Agent with a sample lead.

Verify that it:

Understands the lead requirements.

Produces a useful summary.

Detects site-visit requests.

Uses the Calendar tool when appropriate.

Returns a concise result.

📅 Step 10 — Test Google Calendar

Use a test customer and request a site visit.

Example:

I would like to visit tomorrow at 11 AM.

Check that the AI Agent calls the Calendar tool and that the event appears in your selected calendar.

Test date/time handling carefully before using this workflow for real customers. Natural-language dates such as "tomorrow" depend on timezone and the execution environment.

📊 Step 11 — Test Google Sheets

After a successful AI Agent execution, verify that a new row is added.

Example:

John
01712345678
3 Bed 2 Bath Apartment
1.2 Crore BDT
Uttara
Tomorrow at 11 AM
Lead wants a 3 bedroom apartment...
10/1/2026, 10:30:00 AM
john@example.com

📧 Step 12 — Test Email Notifications

Verify both email steps:

Admin

New Real Estate Lead

The admin should receive the complete lead information.

Customer

The customer should receive an inquiry confirmation.

Before going live, replace any testing/static recipient configuration with the appropriate dynamic customer email field.

🟢 Step 13 — Activate the Workflow

After all tests pass:

Save the workflow.

Confirm all credentials are connected.

Confirm Google Sheet access.

Confirm Calendar access.

Confirm Gmail access.

Confirm the Vapi webhook URL.

Activate the workflow.

Your production flow becomes:

Customer Call
      ↓
Voice AI
      ↓
n8n Webhook
      ↓
Lead Extraction
      ↓
AI Processing
      ↓
Google Sheets
      ↓
Admin Notification
      ↓
Customer Confirmation
      ↓
Calendar Scheduling

🔒 Security & GitHub Safety

This repository is intended to contain a GitHub-safe workflow export.

The cleaned workflow removes:

Stored n8n credential blocks

Credential IDs/names from the exported nodes

Pinned webhook/test data

However, removing credentials does not automatically mean every value inside a workflow is public-safe.

Before making the repository public, review:

Email addresses

Google Sheet IDs and URLs

Webhook URLs

Vapi assistant IDs

Organization IDs

Call IDs

Customer names

Customer phone numbers

Customer email addresses

Conversation transcripts

Any provider-specific identifiers

Never commit:

API keys
Access tokens
OAuth client secrets
Passwords
Private webhook URLs
Service-account JSON files
.env files
Customer PII
Production credentials

📁 Recommended Repository Structure

A professional repository can use:

real-estate-voice-ai-automation/
│
├── workflows/
│   └── Real-Estate-Voice-Assistant-GitHub-Safe.json
│
├── README.md
│
├── .gitignore
│
└── LICENSE

If you keep only the workflow and README, a simpler structure is also fine:

real-estate-voice-ai-automation/
│
├── Real-Estate-Voice-Assistant-GitHub-Safe.json
└── README.md

⚙️ Customization Guide

Change the Company Name

The email templates currently use a real-estate company/assistant presentation.

Search the workflow for:

ABC Real Estate

and replace it with your own company/brand.

Change the AI Assistant Name

The voice assistant configuration can use a name such as:

Sarah

Replace it with your preferred assistant name.

Add More Locations

The JavaScript extraction logic currently recognizes a limited set of locations.

For example:

if (/uttora|kustora|uttara/i.test(transcriptText)) {
  location = 'Uttara';
} else if (/gulshan/i.test(transcriptText)) {
  location = 'Gulshan';
}

You can add more locations:

else if (/mirpur/i.test(transcriptText)) {
  location = 'Mirpur';
}

For a production system, consider replacing hard-coded location matching with a more flexible extraction approach.

Add More Property Types

The current logic recognizes examples such as:

Apartment
Plot
Commercial Space

You can extend it with:

Villa
Duplex
Office
Shop
Warehouse
Land

Customize Lead Fields

You can add fields such as:

Bedrooms
Bathrooms
Parking
Floor
Preferred Project
Payment Method
Move-in Date
Lead Source
Sales Agent
Lead Status

Then update:

Code node

AI Agent prompt

Google Sheets columns

Admin email template

Customer email template

🧠 How the JavaScript Extraction Works

The Code node performs several operations.

1. Reads the incoming webhook payload

const item = $input.item.json;

2. Finds the transcript

It checks possible transcript locations including:

message.artifact.messages
message.transcript
body.transcript

3. Converts transcript messages to text

The workflow transforms messages into a readable format:

AI: Hello...
User: My name is John...
AI: What property are you looking for?
User: A 3 bedroom apartment...

4. Extracts lead information

Regular expressions and keyword checks are used for:

Name

Location

Property

Bedrooms

Bathrooms

Budget

Phone

Email

Site visit

5. Creates an intent summary

The workflow builds an intent string similar to:

Looking for 3 Bed 2 Bath Apartment in Uttara with budget 1.2 Crore BDT

6. Returns structured JSON

That structured data becomes the input for the AI Agent.

🧪 Recommended Test Scenarios

Test the workflow with several different conversations.

Test 1 — Basic Lead

Name: John
Property: Apartment
Location: Uttara
Budget: 1 Crore

Expected:

Lead extracted

Google Sheet row created

Admin notified

Customer confirmation sent

Test 2 — Site Visit

I want a 3 bedroom apartment in Uttara.
My budget is 1.5 crore.
I want to visit tomorrow at 11 AM.

Expected:

Lead extracted

AI Agent detects site visit

Calendar event created

Lead stored

Notifications sent

Test 3 — Missing Information

I am looking for an apartment in Gulshan.

Expected:

Available information is captured

Missing fields remain unavailable rather than being invented

AI Agent can continue the qualification process

Test 4 — Spoken Phone Number

Zero one seven one two three four five six seven eight nine

Verify that the phone extraction logic correctly produces the intended digit sequence.

Test 5 — Spoken Email

john at gmail dot com

Verify that the email normalization produces:

john@gmail.com

🐛 Troubleshooting

Webhook receives data but fields are N/A

Check:

n8n Webhook execution data.

Actual transcript location.

JSON structure from Vapi.

JavaScript extraction rules.

The Code node is based on specific payload structures, so a provider payload change may require code updates.

Google Sheets does not receive data

Check:

Google Sheets credential

Spreadsheet access

Sheet name

Column names

Google account permissions

Make sure your sheet headers match the mapping expected by the workflow.

Gmail fails

Check:

Gmail OAuth credential

Sender account permissions

Recipient email

Gmail node configuration

Whether the Google account allows the requested action

Calendar event is not created

Check:

Google Calendar OAuth credential

Calendar selection

AI Agent tool connection

Site-visit input format

Date/time parsing

n8n server timezone

AI Agent does not call Calendar

Check the AI Agent prompt and confirm that the Calendar node is connected to the AI Agent using an AI Tool connection.

The intended connection is:

Create an event in Google Calendar
              │
              ▼
           AI Agent

Customer email goes to the wrong address

Check the Notify User node.

For production, use the extracted customer email rather than a fixed testing address.

🔐 Production Checklist

Before deploying this workflow for real customers:

Import the workflow.

Connect Gemini credential.

Connect Google Sheets credential.

Connect Gmail credential.

Connect Google Calendar credential.

Create your own Google Sheet.

Configure the correct sheet headers.

Configure your own calendar.

Configure the Vapi/n8n webhook.

Replace company branding.

Replace testing email addresses.

Review the JavaScript extraction rules.

Test phone extraction.

Test email extraction.

Test budget extraction.

Test site-visit scheduling.

Test Google Sheets insertion.

Test admin email.

Test customer email.

Review timezone configuration.

Remove all production/customer test data before publishing the repository.

Confirm no secrets or credentials are committed to GitHub.

Activate the workflow only after end-to-end testing.

📈 Future Improvements

Possible upgrades for a production-grade version:

Lead Scoring

Automatically classify leads by:

Hot
Warm
Cold

based on configurable business rules.

CRM Integration

Send qualified leads to:

HubSpot

Salesforce

Pipedrive

Zoho CRM

WhatsApp Follow-up

Automatically send:

Lead confirmation
Property catalog
Site-visit reminder
Follow-up message

Automated Follow-up

If a lead does not respond:

Day 1 → Follow-up
Day 3 → Reminder
Day 7 → Final follow-up

Sales Assignment

Automatically assign leads to a sales representative based on:

Location
Property type
Budget
Lead availability

Dashboard

Build a dashboard showing:

Total Leads
Today's Leads
Site Visits
Hot Leads
Converted Leads
Pending Follow-ups
Revenue

📚 Project Use Case

This automation is suitable for:

Real estate agencies

Property developers

Property brokers

Real estate call centers

Lead-generation businesses

AI automation portfolios

n8n automation demonstrations

It can also be adapted to other industries where a voice conversation needs to become structured business data.

💡 Why This Project Is Useful

A typical manual process looks like:

Customer Call
     ↓
Sales Person Takes Notes
     ↓
Manually Updates Spreadsheet
     ↓
Sends Internal Message
     ↓
Sends Customer Email
     ↓
Creates Calendar Event

This automation changes the process to:

Customer Call
     ↓
AI Voice Assistant
     ↓
n8n Automation
     ↓
Structured Lead
     ├── Google Sheets
     ├── Admin Notification
     ├── Customer Confirmation
     └── Calendar Event

This reduces repetitive manual work and creates a consistent lead-processing pipeline.

👨‍💻 Author

Md Nahid

AI Automation / Full-Stack Developer

⭐ Project Summary

Real Estate Voice AI Automation is an n8n-based workflow that connects voice AI, LLM processing, structured lead extraction, spreadsheet storage, email automation, and calendar scheduling into a single real-estate lead-management pipeline.

Vapi
  +
n8n
  +
Google Gemini
  +
Google Sheets
  +
Gmail
  +
Google Calendar
  =
Automated Real Estate Lead Management

If this project is useful, consider giving the repository a ⭐ on GitHub.
