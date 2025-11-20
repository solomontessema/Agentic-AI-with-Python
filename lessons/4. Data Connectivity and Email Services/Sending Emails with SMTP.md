4.3: Sending Emails with SMTP

Learning Objectives

By the end of this unit, the reader will be able to:

Configure SMTP for sending emails through a Python script

Format MIME-compliant email messages

Attach summaries, charts, or text to outgoing emails

Integrate email sending into the agentic assistant

SMTP for Automated Agent Responses

SMTP (Simple Mail Transfer Protocol) is the standard for sending emails across networks. For agentic systems, SMTP enables outbound communication, allowing agents to:

Send summaries of recent activity

Deliver query responses or visual reports

Notify users of task completion

By coupling IMAP (for reading) and SMTP (for sending), the agent becomes a complete email-aware system.

Email Sending Workflow

The process of sending an email using SMTP involves:

Connecting to the SMTP server (e.g., smtp.gmail.com)

Logging in using a secure app password

Constructing the email content using MIME

Sending the email via the SMTP connection

Python provides smtplib and email.mime modules for this functionality.

Basic email sending example:


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

This workflow enables agents to communicate results, updates, and reports