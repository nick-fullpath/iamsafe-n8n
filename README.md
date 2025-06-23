# n8n-iamsafe

## Overview

This repository contains a suite of n8n workflow automations for organizational safety check-ins and emergency response, designed to be easily adapted for any company. The workflows leverage Slack, Google Sheets, and (optionally) OpenAI for automated communication and tracking of employee safety status during emergencies.

---

## Workflows Included

### 1. IAMSAFE - Main Automation
- **Purpose:** Monitors an RSS feed for emergency events (e.g., rocket attacks), extracts impacted city names, and triggers notifications to employees in those locations.
- **Key Integrations:**
  - RSS Feed (customizable URL)
  - Google Sheets (employee address book)
  - Slack (employee notifications)
  - OpenAI (optional, for city name extraction from Hebrew text)

### 2. IAMSAFE - Cities Manual Add Automation
- **Purpose:** Allows manual addition of impacted cities and notifies employees in those locations.
- **Key Integrations:**
  - Google Sheets (city and employee data)
  - Slack (employee notifications)
  - OpenAI (optional, for city name extraction)

### 3. IAMSAFE - Send Reminders
- **Purpose:** Periodically reminds the HR Channel about employees who have not responded to safety check-ins.
- **Key Integrations:**
  - Google Sheets (track responses)
  - Slack (reminders)

### 4. IAMSAFE - Slack i-am-safe
- **Purpose:** Listens for updates in the Google Sheet and sends Slack check-in messages to employees in impacted areas.
- **Key Integrations:**
  - Google Sheets (trigger on status change)
  - Slack (direct messages to users)

### 5. IAMSAFE - Slack Response from User
- **Purpose:** Handles Slack button responses from employees, updates their status in Google Sheets, and notifies HR.
- **Key Integrations:**
  - Slack (interactive messages)
  - Google Sheets (update status)

### 6. IAMSAFE - SlackID to Email Mapper
- **Purpose:** Maps Slack user IDs to email addresses and updates the Google Sheet for accurate employee matching. (This should be triggered manually only when new employees are added to the Google Sheet)
- **Key Integrations:**
  - Slack (fetch user list)
  - Google Sheets (update mapping)

### 7. IAMSAFE - Manager Update Flow
- **Purpose:** Notifies an employee when his manager changes his status for him.
- **Key Integrations:**
  - Google Sheets (trigger on status change)
  - Slack (manager notifications)

---

## Required Credentials & Permissions

To use these workflows, you must configure the following credentials in n8n:

### 1. **Google Sheets**
- **Credential Type:** OAuth2
- **Required Scopes:** `https://www.googleapis.com/auth/spreadsheets`
- **Permissions:**
  - Read and write access to the employee address book and city sheets.
- **Setup:**
  - Create a Google Cloud project and OAuth2 client.
  - Share the relevant Google Sheets with the service account or user.
  - In n8n, add a new credential for Google Sheets OAuth2 and connect it to your account.

### 2. **Slack**
- **Credential Types:**
  - Slack OAuth2 (for user-level actions, e.g., fetching users)
  - Slack API Token (for bot actions, e.g., sending messages)
- **Required Scopes:**
  - `users:read` (fetch user list)
  - `chat:write` (send messages)
  - `channels:read` (read channel info)
  - `commands` (for interactive messages)
- **Setup:**
  - Create a Slack App in your workspace.
  - Add the above scopes and install the app to your workspace.
  - In n8n, add new credentials for Slack OAuth2 and/or Slack API Token.

### 3. **OpenAI (Optional)**
- **Credential Type:** OpenAI API Key
- **Permissions:**
  - Access to GPT-3.5/4 models for city name extraction from text (optional, for advanced parsing).
- **Setup:**
  - Create an OpenAI account and generate an API key.
  - In n8n, add a new credential for OpenAI API.

---

## Dynamic Value Replacement

To adapt these workflows for your organization, you must replace hardcoded values with your own:

- **Google Sheets Document IDs & Sheet Names:**
  - Replace all `documentId` and `sheetName` values with your own Google Sheet IDs and sheet/tab names.

- **Slack Channel IDs:**
  - Replace all `channelId` values (e.g., `C0918AFJ2J3`) with your own HR Slack channel ID.
- **Slack User IDs:**
  - If you use direct user notifications, ensure user IDs are dynamically mapped from your Slack workspace (Use the "IAMSAFE - SlackID to Email Mapper" workflow to help you achive this).
- **Email Domains:**
  - Update any email domain logic to match your organization (e.g., `@fullpath.com` → your domain).
- **RSS Feed URL:**
  - Update the RSS feed URL in the Main Automation to a relevant source for your region or use case.

---

## Setup Instructions

1. **Import Workflows:**
   - In n8n, import each JSON file as a new workflow.
2. **Import Sheets and Data:**
   - Copy the Google Sheets examples from the "example_sheets" folder and populate the "example_employees_list" with your employee data. *City names must be in "Hebrew" to work with "Hebrew" RSS Feed source.
3. **Configure Credentials:**
   - Set up Google Sheets, Slack, and (optionally) OpenAI credentials on n8n as described above.
4. **Update Dynamic Values:**
   - Edit each workflow to replace document IDs, sheet names, channel IDs, and any other organization-specific values.
5. **Share Google Sheets:**
   - Ensure the Google Sheets are shared with the account used for the OAuth2 credential.
6. **Test Each Workflow:**
   - Use manual triggers or test events to verify each workflow end-to-end.
7. **Activate Workflows:**
   - Once tested, activate the workflows in n8n for production use.

---