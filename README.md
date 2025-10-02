job reply - This n8n workflow monitors Gmail every minute for new emails, uses AI to determine if they're job opportunities, then auto-generates and sends personalized replies highlighting Leela's experience, certifications, and contact details. It also sends notification emails about detected job opportunities before replying.
<img width="1600" height="558" alt="image" src="https://github.com/user-attachments/assets/1511f920-b617-4143-9bcd-0df13f2d0164" />

Here's the technical workflow breakdown:
1. Gmail Trigger Node

Polls Gmail every minute using OAuth2 authentication
Retrieves new emails with headers, subject, body text

2. Edit Fields Node

Extracts and formats email data (from, subject, body) into a structured "email context" string variable

3. AI Agent Node

Sends email context to OpenAI GPT-4.1-mini
Uses a defined prompt asking if email is job-related
Expects JSON response with isJobOpportunity (true/false) and reasoning

4. Structured Output Parser

Validates AI response matches expected JSON schema
Ensures consistent data structure for downstream nodes

5. Send a Message Node

Sends notification email to leelakrishna.sarepalli@gmail.com
Contains original email details and AI reasoning

6. If Conditional Node

Checks if isJobOpportunity === "true"
Routes workflow: job opportunity → continue, not job → terminate

7. Message a Model Node

Second OpenAI call with detailed system prompt
Generates personalized HTML email body matching job requirements
Pulls from Leela's experience, education, tools, certifications

8. Reply to Message Node

Uses Gmail API to reply to original email thread
Sends AI-generated response with sender name customization
replyToSenderOnly: true ensures direct reply

Data Flow: Gmail → Format → AI Classification → Notification → Conditional Check → AI Response Generation → Gmail Reply
