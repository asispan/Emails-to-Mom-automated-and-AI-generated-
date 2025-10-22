# Cron Email to Mom

**An intelligent n8n workflow that sends personalized daily news digests to your loved ones**

[![n8n](https://img.shields.io/badge/n8n-Workflow-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)](https://n8n.io)
[![OpenAI](https://img.shields.io/badge/OpenAI-GPT--4-412991?style=for-the-badge&logo=openai&logoColor=white)](https://openai.com)
[![Gmail](https://img.shields.io/badge/Gmail-Integration-D14836?style=for-the-badge&logo=gmail&logoColor=white)](https://gmail.com)
[![Status](https://img.shields.io/badge/Status-Active-success?style=for-the-badge)](https://github.com)

[Features](#features) | [Architecture](#architecture) | [Installation](#installation) | [Configuration](#configuration) | [Usage](#usage)

<p align="left">
  <br>
  <img src="https://github.com/asispan/Emails-to-Mom-automated-and-AI-generated-/blob/main/workflow-example.png" alt="n8n Workflow Visualization" width="100%">
  <br>
  <br>
</p>

---

## Overview

**Cron Email to Mom** is an automated n8n workflow designed to keep your family informed with curated news content. The workflow leverages AI-powered news aggregation to fetch, translate, and deliver personalized news updates via email at scheduled times throughout the day.

### Use Case

Stay connected with family members by automatically sending them:
- **Regional News**: Top 5 news stories from Odisha (in Odia language)
- **National News**: Top 5 significant news from India
- **International News**: Top 5 global headlines (excluding India)
- **Fun Facts & Knowledge**: Daily interesting facts, historical events, and science updates

---

## Features

### AI-Powered Content Generation
- Utilizes OpenAI GPT-4 for intelligent news curation
- Structured output parsing for consistent formatting
- Context-aware content summarization

### Multi-Language Support
- Automatic translation from English to Odia
- Google Translate API integration
- Preserves formatting across languages

### Scheduled Execution
- **8:02 AM**: Odisha regional news (in Odia)
- **12:01 PM**: Indian national news
- **4:14 PM**: International news
- **8:12 PM**: Fun facts and interesting knowledge

### Comprehensive Logging
- All sent emails logged to Google Sheets
- Tracks date, time, subject, and content
- Error handling and status monitoring

### Professional Email Delivery
- Gmail integration with OAuth2 authentication
- HTML-formatted emails with rich content
- Reply tracking enabled

---

## Architecture

### Workflow Components

```
+------------------+
| Schedule Trigger | (4 different times)
+--------+---------+
         |
         v
+------------------+
|   AI Agent       | (OpenAI GPT-4)
|  News Curation   |
+--------+---------+
         |
         v
+------------------+
|  Translation     | (Google Translate - for Odia)
+--------+---------+
         |
         v
+------------------+
| Text Processing  | (Paragraph chunking)
+--------+---------+
         |
         v
+------------------+
|  Email Sending   | (Gmail)
+--------+---------+
         |
         v
+------------------+
|   Logging        | (Google Sheets)
+------------------+
```

### Key Nodes

| Node Type | Purpose | Configuration |
|-----------|---------|---------------|
| **Schedule Trigger** | Time-based workflow initiation | 4 triggers at different times |
| **AI Agent** | News content generation | GPT-4-mini for efficiency |
| **OpenAI Chat Model** | LLM processing | Temperature: 0.7 |
| **Structured Output Parser** | JSON schema-based parsing | Custom news format schema |
| **Google Translate** | Language translation | English to Odia (or) |
| **Code Node** | Text chunking & formatting | JavaScript-based processing |
| **Gmail Node** | Email delivery | OAuth2 authenticated |
| **Google Sheets** | Audit logging | Append operation |

---

## Installation

### Prerequisites

- n8n instance (self-hosted or cloud)
- OpenAI API account with GPT-4 access
- Google Workspace account (for Gmail and Sheets)
- Google Cloud project with enabled APIs:
  - Gmail API
  - Google Sheets API
  - Google Translate API

### Step 1: Clone or Download Workflow

```bash
# Download the sanitized workflow JSON
wget https://github.com/asispan/Emails-to-Mom-automated-and-AI-generated-/raw/main/Cron-Email-to-Mom-Sanitized.json
```

### Step 2: Import into n8n

1. Open your n8n instance
2. Click on **Workflows** -> **Import from File**
3. Select `Cron-Email-to-Mom-Sanitized.json`
4. The workflow will be imported with placeholder credentials

---

## Configuration

### 1. OpenAI Credentials

Create an OpenAI credential in n8n:

1. Navigate to **Credentials** -> **New**
2. Select **OpenAI API**
3. Add your API key
4. Name it appropriately (e.g., "OpenAI Account")

**Update the following nodes:**
- OpenAI Chat Model
- OpenAI Chat Model1
- OpenAI Chat Model2
- OpenAI Chat Model3

### 2. Gmail OAuth2 Credentials

Configure Gmail OAuth2:

1. Create credentials in **Google Cloud Console**
2. Add OAuth2 credentials in n8n:
   - Client ID
   - Client Secret
   - Authorize with your Google account
3. Name it (e.g., "Gmail Account")

**Update the following nodes:**
- Email Sent
- Email Sent1
- Email Sent2
- Email Sent3

**Replace placeholder email:**
```json
"sendTo": "<RECIPIENT_EMAIL_ADDRESS>"
```
with your recipient's email address (e.g., "mom@example.com")

### 3. Google Sheets Credentials

Configure Google Sheets OAuth2:

1. Use the same Google Cloud project
2. Add Google Sheets OAuth2 credentials in n8n
3. Authorize with your Google account

**Update the "Logs" node:**
- Replace `<GOOGLE_SHEETS_URL>` with your Google Sheets URL
- Ensure the sheet has columns: Date, Time, Subject, Email Content, Issue

### 4. Google Translate Credentials

Configure Google Translate:

1. Enable Google Translate API in Google Cloud
2. Create OAuth2 credentials in n8n
3. Authorize with your Google account

**Update the node:**
- English to Odia Translation

### 5. Customize Content

#### Email Content
Modify the email messages in the "Email Sent" nodes to personalize:
- Subject lines
- Email body text
- Salutations and signatures

#### News Sources
Adjust AI Agent prompts to customize:
- News topics
- Geographic regions
- Number of stories
- Summary length

#### Schedule Times
Modify Schedule Trigger nodes to change when emails are sent:
- Current: 8:02 AM, 12:01 PM, 4:14 PM, 8:12 PM (IST)
- Adjust based on recipient's timezone and preferences

---

## Usage

### Activating the Workflow

1. Open the workflow in n8n
2. Ensure all credentials are properly configured
3. Click the **Active** toggle in the top-right corner
4. The workflow will now run automatically at scheduled times

### Testing the Workflow

Before activating, test individual branches:

1. Click on a Schedule Trigger node
2. Click **Execute Node**
3. Verify the output at each step
4. Check that emails are received
5. Confirm logging in Google Sheets

### Monitoring

Monitor workflow execution:
- **Executions** tab shows all workflow runs
- Check Google Sheets for email delivery logs
- Review n8n execution logs for errors

### Manual Execution

To send an email immediately:
1. Click on any Schedule Trigger node
2. Click **Execute Node**
3. The workflow will run for that specific branch

---

## Workflow Details

### Workflow 1: Odisha Regional News (8:02 AM)

1. **Trigger**: Schedule at 8:02 AM IST
2. **AI Agent**: Fetches top 5 Odisha news from last 24 hours
3. **Translation**: Converts English to Odia
4. **Formatting**: Chunks text into paragraphs
5. **Email**: Sends formatted email in Odia
6. **Logging**: Records in Google Sheets

### Workflow 2: Indian National News (12:01 PM)

1. **Trigger**: Schedule at 12:01 PM IST
2. **AI Agent**: Fetches top 5 national news
3. **Formatting**: Chunks and formats content
4. **Email**: Sends English news email
5. **Logging**: Records in Google Sheets

### Workflow 3: International News (4:14 PM)

1. **Trigger**: Schedule at 4:14 PM IST
2. **AI Agent**: Fetches top 5 international news (excluding India)
3. **Formatting**: Processes content
4. **Email**: Delivers formatted email
5. **Logging**: Records in Google Sheets

### Workflow 4: Fun Facts & Knowledge (8:12 PM)

1. **Trigger**: Schedule at 8:12 PM IST
2. **AI Agent**: Generates 5 interesting facts/knowledge items:
   - This Day That Year
   - Fun Fact
   - General Knowledge
   - Did You Know
   - Latest in Science
3. **Email**: Sends HTML-formatted email with headings
4. **Logging**: Records in Google Sheets

---

## Customization Guide

### Adding New News Categories

1. Duplicate an existing AI Agent node
2. Modify the prompt to specify new category
3. Connect to appropriate formatting and email nodes
4. Update email subject and content

### Changing Translation Languages

Modify the Google Translate node:
```json
"translateTo": "or"  // Change "or" to desired language code
```

Language codes: `hi` (Hindi), `ta` (Tamil), `te` (Telugu), etc.

### Adjusting AI Model

Change the OpenAI model in Chat Model nodes:
```json
"model": {
  "value": "gpt-4.1-mini"  // Options: gpt-4o, gpt-4-turbo, etc.
}
```

### Email Formatting

Customize email templates in Gmail nodes:
- Plain text: Use `\n` for line breaks
- HTML: Use HTML tags (`<h2>`, `<b>`, `<br>`)

---

## Troubleshooting

### Common Issues

**Issue: Credentials not working**
- Solution: Re-authorize OAuth2 credentials in n8n
- Ensure APIs are enabled in Google Cloud Console

**Issue: Emails not sending**
- Solution: Check Gmail API quotas
- Verify recipient email address
- Check spam folder

**Issue: AI responses are empty**
- Solution: Verify OpenAI API key is valid
- Check API usage limits
- Ensure prompts are clear and specific

**Issue: Translation errors**
- Solution: Check Google Translate API is enabled
- Verify text length is within limits
- Ensure source text is in English

**Issue: Google Sheets not logging**
- Solution: Verify sheet URL is correct
- Check column names match exactly
- Ensure OAuth2 has sheets.write permission

### Error Workflow

The workflow includes error handling:
- Failed executions are logged
- Error workflow can be configured in settings
- Email continues on error (continueErrorOutput enabled)

---

## Logs and Analytics

### Google Sheets Log Structure

| Column | Description | Example |
|--------|-------------|---------|
| Date | Email sent date | 22/10/2025 |
| Time | Email sent time | 12:01:52 PM |
| Subject | Email subject line | "Good Morning Mom..." |
| Email Content | Full email body | News content |
| Issue | Label/status | SENT/ERROR |

### Monitoring Recommendations

- Review Google Sheets weekly for delivery status
- Check n8n execution history for errors
- Monitor OpenAI API usage and costs
- Set up alerts for failed executions

---

## Security Best Practices

### Credential Management
- Never commit actual credentials to version control
- Use n8n's credential encryption
- Rotate API keys regularly
- Limit OAuth2 scopes to minimum required

### Data Privacy
- Sanitize workflow exports before sharing
- Remove personal email addresses from exports
- Clear pinned test data before exporting
- Be cautious with news content and copyrights

### API Security
- Set up API usage alerts
- Monitor for unusual activity
- Use environment-specific credentials
- Enable 2FA on all accounts

---

## License

This workflow is provided as-is for personal use. Please respect:
- OpenAI's usage policies
- Google's API terms of service
- News content copyrights
- Email anti-spam regulations

---

## Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

### Ideas for Contributions
- Additional language translations
- More news categories
- Weather integration
- RSS feed support
- Email digest templates
- Mobile-friendly formatting

---

## Support

### Getting Help
- **n8n Community**: [community.n8n.io](https://community.n8n.io)
- **n8n Documentation**: [docs.n8n.io](https://docs.n8n.io)
- **OpenAI Support**: [help.openai.com](https://help.openai.com)

### Reporting Issues
Please open an issue with:
- n8n version
- Node versions
- Error messages
- Steps to reproduce

---

## Acknowledgments

- **n8n** - Workflow automation platform
- **OpenAI** - GPT-4 language model
- **Google** - Gmail, Sheets, and Translate APIs
- **Community** - Inspiration and support

---

## Roadmap

Future enhancements planned:
- [ ] SMS notifications via Twilio
- [ ] WhatsApp integration
- [ ] Voice message generation
- [ ] Image/infographic generation
- [ ] RSS feed aggregation
- [ ] Sentiment analysis
- [ ] Custom news sources
- [ ] Multi-recipient support
- [ ] Dashboard for analytics
- [ ] Mobile app integration

---

**Made with dedication for keeping families connected**

If you found this workflow helpful, please star the repository!
