# API Research

## Gmail API

### Core Findings
- **Daily Quotas**: 
  - Per user: 250 units/second (~50 emails/second) - [Source](https://developers.google.com/gmail/api/reference/quota)
  - Daily total: 1 billion quota units per day - [Source](https://mailtrap.io/blog/send-emails-with-gmail-api/)
  - Different operations cost different quota units:
    - messages.get: 5 units - [Source](https://developers.google.com/gmail/api/reference/quota)
    - messages.list: 5 units - [Source](https://developers.google.com/gmail/api/reference/quota)
    - labels.get: 1 unit - [Source](https://developers.google.com/gmail/api/reference/quota)
    - labels.list: 1 unit - [Source](https://developers.google.com/gmail/api/reference/quota)

- **Authentication**: 
  - Requires OAuth 2.0 (not API keys) - [Source](https://developers.google.com/identity/protocols/oauth2)
  - Credentials obtained from Google API Console - [Source](https://developers.google.com/identity/protocols/oauth2)

- **Required Scope**: 
  - `https://www.googleapis.com/auth/gmail.readonly` for read-only access - [Source](https://developers.google.com/gmail/api/quickstart/python)

### Implementation Steps
1. **Set up a Google Cloud Project**:
   - Create project in Google Cloud Console - [Source](https://developers.google.com/gmail/api/quickstart/python)
   - Enable Gmail API - [Source](https://developers.google.com/gmail/api/quickstart/python)
   - Configure OAuth consent screen - [Source](https://developers.google.com/identity/protocols/oauth2)
   - Create OAuth 2.0 client ID credentials - [Source](https://developers.google.com/identity/protocols/oauth2)

2. **OAuth 2.0 Implementation**:
   - Using `google-auth-oauthlib` library - [Source](https://stackoverflow.com/questions/65695786/gmail-api-how-to-simply-authenticate-a-user-and-get-a-list-of-their-messages)
   - Store credentials in a token file (e.g., token.json) - [Source](https://developers.google.com/gmail/api/quickstart/python)
   - Token refresh handled by library - [Source](https://developers.google.com/gmail/api/quickstart/python)

3. **Accessing Labeled Emails**:
   - Can filter emails by label using `labelIds` parameter - [Source](https://developers.google.com/gmail/api/guides/filtering)
   - Example: `service.users().messages().list(userId='me', labelIds=['INBOX']).execute()` - [Source](https://stackoverflow.com/questions/60750345/gmail-api-unable-to-list-all-mails-by-labels)
   - For "Newsletters" label, need to first get the ID using `labels().list()` - [Source](https://developers.google.com/gmail/api/reference/rest/v1/users.labels/get)

### Important Considerations
- **Token Storage**: 
  - Securely store refresh tokens - [Source](https://developers.google.com/identity/protocols/oauth2)
  - Token expiration: access tokens expire, refresh tokens can last indefinitely if used - [Source](https://developers.google.com/identity/protocols/oauth2)

- **Rate Limiting**: 
  - Implement exponential backoff for retries - [Unverified - Need to research]
  - Monitor quota usage in Google Cloud Console - [Unverified - Need to research]

### Questions for Further Research
- Exact format of email metadata returned by the API
- Testing with sample newsletter emails to understand structure
- Handling of HTML vs. plain text content in newsletters
- Best practices for implementing exponential backoff
- How to extract links and format content for newsletter generation

### Resources
- [Gmail API Python Quickstart](https://developers.google.com/gmail/api/quickstart/python)
- [Gmail API Usage Limits](https://developers.google.com/gmail/api/reference/quota)
- [OAuth 2.0 for Google APIs](https://developers.google.com/identity/protocols/oauth2)
- [Searching for Messages](https://developers.google.com/gmail/api/guides/filtering)