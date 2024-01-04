---
layout: post
title:  "Summarizing your Gmail inbox newsletters using OpenAI"
date:   2024-01-04 01:00:00 -0300
categories: 'insight'
---

As I've written on a [LinkedIn post](https://www.linkedin.com/posts/nicolasjengler_ai-informationoptimization-professionalefficiency-activity-7148654214834020352-CapQ), lately I've been using OpenAI's ChatGPT to summarize some blog posts and most of the newsletters I receive on my main email address. It has been a great experience as it allows me to efficiently consume the main discussion. I can also expand or dive deeper if it interests me enough.

I decided to try and automate the process as much as possible and created this proof-of-concept that attempts to retrieve emails from your Gmail inbox and summarize them using OpenAI's API. DISCLAIMER: The script is written in Python and uses pip to manage dependencies. However, I'm almost certain it can be adapted to a JavaScript/Node-based environment if needed.

---

To create a command-line application that fetches emails from Gmail and summarizes them using OpenAI's API, you'll need to follow the following steps. We'll be using the `google-auth` library for Gmail authentication and the `openai` library for ChatGPT API access. Please note that you'll need to install these libraries first using `pip`:

```bash
pip install google-auth google-auth-oauthlib google-auth-httplib2 openai
```

Now, let's create a basic script. Ensure you have the required API keys and credentials for Gmail and OpenAI.

```python
import os
import openai
from google.oauth2.credentials import Credentials
from google_auth_oauthlib.flow import InstalledAppFlow
from google.auth.transport.requests import Request
import googleapiclient.discovery

# Set up Google API credentials, these will be stored as JSON files that for simplicity's sake will live in the root directory of the project
SCOPES = ['https://www.googleapis.com/auth/gmail.readonly']
API_TOKEN_PATH = 'token.json'
CREDENTIALS_PATH = 'credentials.json'

# OpenAI
OPENAI_API_KEY = 'YOUR_OPENAI_API_KEY'

def authenticate_gmail():
    creds = None

    if os.path.exists(API_TOKEN_PATH):
        creds = Credentials.from_authorized_user_file(API_TOKEN_PATH)

    if not creds or not creds.valid:
        if creds and creds.expired and creds.refresh_token:
            creds.refresh(Request())
        else:
            flow = InstalledAppFlow.from_client_secrets_file(CREDENTIALS_PATH, SCOPES)
            creds = flow.run_local_server(port=0)

        with open(API_TOKEN_PATH, 'w') as token:
            token.write(creds.to_json())

    return creds

def get_emails():
    creds = authenticate_gmail()
    service = googleapiclient.discovery.build('gmail', 'v1', credentials=creds)

    results = service.users().messages().list(userId='me', labelIds=['INBOX']).execute()
    messages = results.get('messages', [])

    if not messages:
        print('No messages found.')
        return []

    return messages

def summarize_email(email_body):
    openai.api_key = OPENAI_API_KEY
    
    prompt = f"Please, summarize the following newsletter:\n{email_body}"
    
    # Adjust your OpenAI settings when processing content here
    # These are settings specific to OpenAI so check their docs if you want further clarity
    response = openai.Completion.create(
        engine="text-davinci-003",
        prompt=prompt,
        max_tokens=150,
        temperature=0.7
    )

    return response.choices[0].text.strip()

def main():
    emails = get_emails()

    for email in emails:
        message = service.users().messages().get(userId='me', id=email['id']).execute()
        email_body = message['snippet']  # You may need to adjust this based on the email structure

        summary = summarize_email(email_body)
        # Uncomment the following line if you want the full original email to show right before getting the summary
        # print(f"Original Email:\n{email_body}\n")
        print(f"Summarized Email:\n{summary}\n")
        print("="*50)

if __name__ == '__main__':
    main()
```

Obviously feel free to adjust the code according to your specific use case, especially when handling email bodies and formatting (this can prove tricky as formats can vary wildly, I've noticed this to be more efficient with LinkedIn community newsletters).

I hope you put this weird little experiment to good use!