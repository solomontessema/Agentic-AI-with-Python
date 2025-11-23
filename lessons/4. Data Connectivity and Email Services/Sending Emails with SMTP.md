4.3: Sending Emails with SMTP

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Data Connectivity and Email Services/Email_Sender_Module.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

*Explore further in the accompanying* **podcast episode**

<a href="https://copilot.microsoft.com/shares/podcasts/MN28rgMgUnuuTcwy5xBCPph" target="_blank" rel="noopener noreferrer" 
   style="display:inline-block; background:#0078D4; color:white; padding:2px 24px; border-radius:6px; text-decoration:none; font-weight:600;">
🎧  Listen 
   </a>

 ---
### SMTP for Automated Agent Responses

SMTP (Simple Mail Transfer Protocol) is the standard for sending emails across networks. For agentic systems, SMTP enables outbound communication, allowing agents to:

- Send summaries of recent activity

- Deliver query responses or visual reports

- Notify users of task completion

By coupling IMAP (for reading) and SMTP (for sending), the agent becomes a complete email-aware system.

----

### Email Sending Workflow

The process of sending an email using SMTP involves:

1. Connecting to the SMTP server (e.g., smtp.gmail.com)

2. Logging in using a secure app password

3. Constructing the email content using MIME

4. Sending the email via the SMTP connection

Python provides ```smtplib``` and ```email.mime``` modules for this functionality.

**Basic email sending example:**

```python
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart

# Email credentials and server setup
smtp_server = "smtp.gmail.com"
smtp_port = 587
sender_email = "your_email@gmail.com"
receiver_email = "recipient@example.com"
password = "your_app_password"

# Create the email content
message = MIMEMultipart()
message["From"] = sender_email
message["To"] = receiver_email
message["Subject"] = "Automated Agent Report"

body = "Here is the summary of recent activity."
message.attach(MIMEText(body, "plain"))

# Send the email
with smtplib.SMTP(smtp_server, smtp_port) as server:
    server.starttls()
    server.login(sender_email, password)
    server.sendmail(sender_email, receiver_email, message.as_string())
```

This workflow enables agents to communicate results, updates, and reports