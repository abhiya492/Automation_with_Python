# Python Email and Text Messaging - Complete Guide

Let me walk through all the topics covered in the chapter on sending emails and text messages with Python:

## SMTP for Sending Emails

### Understanding SMTP
- Simple Mail Transfer Protocol (SMTP) is the standard for sending emails across the internet
- Handles formatting, encryption, relaying between mail servers
- Python's `smtplib` module provides functions to simplify SMTP interaction

### Basic Email Sending Process
```python
import smtplib
smtpObj = smtplib.SMTP('smtp.example.com', 587)
smtpObj.ehlo()
smtpObj.starttls()
smtpObj.login('bob@example.com', 'MY_SECRET_PASSWORD')
smtpObj.sendmail('bob@example.com', 'alice@example.com', 'Subject: Hello.\nDear Alice, message body. Sincerely, Bob')
smtpObj.quit()
```

### Connecting to SMTP Server
- Each email provider has a specific SMTP server domain name and port
- Common providers:
  - Gmail: smtp.gmail.com (port 587)
  - Outlook/Hotmail: smtp-mail.outlook.com (port 587)
  - Yahoo Mail: smtp.mail.yahoo.com (port 587)
  - Others listed in Table 16-1
- Use `smtplib.SMTP(server, port)` to connect with TLS, or `smtplib.SMTP_SSL(server, 465)` for SSL

### SMTP Authentication Process
1. Send "Hello" message with `ehlo()`
2. Start TLS encryption with `starttls()` (skip for SSL connections)
3. Log in with `login(username, password)`
4. Security note: Avoid hardcoding passwords in your script

### Sending Emails
- Use `sendmail(from_email, to_email, message_body)`
- For multiple recipients, use a list of email addresses for the second parameter
- Format the message with 'Subject: Line\n' at the beginning
- An empty dictionary return value means success

### Gmail-Specific Setup
- May require application-specific passwords
- Need to set up in Google Account settings

### Disconnecting
- Always disconnect from SMTP server using `quit()` when finished

## IMAP for Retrieving Emails

### Understanding IMAP
- Internet Message Access Protocol (IMAP) retrieves emails from servers
- Python's built-in `imaplib` is available, but third-party `imapclient` is easier to use
- `pyzmail` helps parse email content into readable formats

### Email Retrieval Process Overview
```python
import imapclient
imapObj = imapclient.IMAPClient('imap.gmail.com', ssl=True)
imapObj.login('my_email_address@gmail.com', 'MY_SECRET_PASSWORD')
imapObj.select_folder('INBOX', readonly=True)
UIDs = imapObj.search(['SINCE 05-Jul-2014'])
rawMessages = imapObj.fetch([40041], ['BODY[]'])
import pyzmail
message = pyzmail.PyzMessage.factory(rawMessages[40041]['BODY[]'])
message.get_subject()
# Access other parts of the message
imapObj.logout()
```

### Connecting to IMAP Server
- Common IMAP servers:
  - Gmail: imap.gmail.com
  - Outlook/Hotmail: imap-mail.outlook.com
  - Yahoo: imap.mail.yahoo.com
  - Others in Table 16-2
- Use `imapclient.IMAPClient(server, ssl=True)` to create connection
- Login with `login(email, password)`

### Searching for Emails
1. Select a folder with `select_folder('INBOX', readonly=True)`
   - `readonly=True` prevents accidental modifications
2. Find folders with `list_folders()` if needed
3. Search with `search([criteria])` using IMAP search keys:
   - 'ALL' for all messages
   - 'BEFORE date', 'ON date', 'SINCE date' for time filtering
   - 'SUBJECT string', 'BODY string', 'TEXT string' for content search
   - 'FROM string', 'TO string', 'CC string', 'BCC string' for address search
   - 'SEEN'/'UNSEEN', 'DELETED'/'UNDELETED' for status filters
   - And many others detailed in Table 16-3

### Size Limits in IMAP
- Default size limit is 10,000 bytes
- Increase with:
  ```python
  import imaplib
  imaplib._MAXLINE = 10000000
  ```

### Gmail-Specific Searching
- Use `gmail_search()` method to utilize Google's search engine 
- Works like the Gmail search bar

### Fetching and Reading Email Content
1. Fetch emails with `fetch(UIDs, ['BODY[]'])`
2. Parse with `pyzmail.PyzMessage.factory()`
3. Extract information:
   - Subject: `message.get_subject()`
   - Addresses: `message.get_addresses('from'/'to'/'cc'/'bcc')`
   - Text content: `message.text_part.get_payload().decode(message.text_part.charset)`
   - HTML content: `message.html_part.get_payload().decode(message.html_part.charset)`

### Deleting Emails
1. Select folder with `readonly=False`
2. Mark messages for deletion with `delete_messages(UIDs)`
3. Permanently delete with `expunge()`

### Disconnecting from IMAP Server
- Always disconnect with `logout()` when finished
- Handle potential timeout errors by reconnecting

## Project: Sending Member Dues Reminder Emails

### Project Overview
- Read data from Excel spreadsheet of membership dues payments
- Find members who haven't paid for latest month
- Send personalized reminder emails

### Implementation Steps
1. Open Excel file with `openpyxl`
2. Find most recent month's column
3. Check each member's payment status
4. Create dictionary of unpaid members
5. Log in to email account
6. Send customized emails to each unpaid member
7. Check for any sending errors

### Code Walkthrough
- Reading Excel data:
  ```python
  wb = openpyxl.load_workbook('duesRecords.xlsx')
  sheet = wb.get_sheet_by_name('Sheet1')
  lastCol = sheet.get_highest_column()
  latestMonth = sheet.cell(row=1, column=lastCol).value
  ```

- Finding unpaid members:
  ```python
  unpaidMembers = {}
  for r in range(2, sheet.get_highest_row() + 1):
      payment = sheet.cell(row=r, column=lastCol).value
      if payment != 'paid':
          name = sheet.cell(row=r, column=1).value
          email = sheet.cell(row=r, column=2).value
          unpaidMembers[name] = email
  ```

- Sending emails:
  ```python
  for name, email in unpaidMembers.items():
      body = "Subject: %s dues unpaid.\nDear %s,\nRecords show that you have not paid dues for %s. Please make this payment as soon as possible. Thank you!'" % (latestMonth, name, latestMonth)
      print('Sending email to %s...' % email)
      sendmailStatus = smtpObj.sendmail('my_email_address@gmail.com', email, body)
      if sendmailStatus != {}:
          print('There was a problem sending email to %s: %s' % (email, sendmailStatus))
  ```

## Sending Text Messages with Twilio

### Twilio Setup Process
1. Sign up for a Twilio account
2. Verify your mobile phone number
3. Get a Twilio phone number
4. Record your account SID and authentication token
5. Install the `twilio` module

### Sending Texts
```python
from twilio.rest import TwilioRestClient
accountSID = 'ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
authToken = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
twilioCli = TwilioRestClient(accountSID, authToken)
myTwilioNumber = '+14955551234'
myCellPhone = '+14955558888'
message = twilioCli.messages.create(body='Hello!', from_=myTwilioNumber, to=myCellPhone)
```

### Checking Message Status
- Initial status is 'queued'
- Get updated status:
  ```python
  updatedMessage = twilioCli.messages.get(message.sid)
  updatedMessage.status  # 'delivered', 'sent', etc.
  updatedMessage.date_sent
  ```

### Limitations of Receiving SMS
- Requires web application setup
- More complicated than sending messages

## Project: "Just Text Me" Module

### Project Overview
- Create reusable module to text yourself notifications

### Implementation
```python
# textMyself.py
accountSID = 'ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
authToken = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
myNumber = '+15559998888'
twilioNumber = '+15552225678'

from twilio.rest import TwilioRestClient

def textmyself(message):
    twilioCli = TwilioRestClient(accountSID, authToken)
    twilioCli.messages.create(body=message, from_=twilioNumber, to=myNumber)
```

### Usage
```python
import textmyself
textmyself.textmyself('The boring task is finished.')
```

## Practice Projects

The chapter concludes with suggested practice projects:

1. **Random Chore Assignment Emailer**
   - Randomly assign chores to people
   - Send emails with assignments

2. **Umbrella Reminder**
   - Check weather forecasts
   - Text reminder to bring umbrella if raining

3. **Auto Unsubscriber**
   - Scan emails for unsubscribe links
   - Open links automatically in browser

4. **Computer Control Through Email**
   - Check email for instructions
   - Execute commands on your computer
   - Send confirmation when completed

These projects combine the email/SMS techniques with other Python skills covered elsewhere in the book.
