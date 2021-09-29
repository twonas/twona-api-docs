# Feedback and Approvals

- [Terminology](#terminology)
- [Share a feedback (POST)](#share-an-approval)

## Terminology

### Feedback

**Feedback**: this feature allows the users to request feedback in the form of notes on files, comments and file upload, aswell as formal approval/rejections with specific reasons. You can specify a collection of email addresses to which an email requesting the feedback will be sent along with a link to provide such feedback.

## Request Feedback

Send a request for feedback/approval to a collection of email addresses (can be users in your organisation, but it is not a requirement, the feature will also send the request to outside email addresses).

### Request
#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/p/feedback

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**emails** (required) | array | Collection of emails to send the request for feedback/approval to
**subject** (required) | string | Short explanation that will be used in the email Subject
**message** (required) | string | Long text to include further explanations or details
**files** (required) | array<integer> | Collection of file IDs to get feedback/approval from
**allow_see** (optional) | boolean | TRUE to allow a transparent process where users see each other's feedback/approvals (default: `false`)
**get_approval** (optional) | boolean | TRUE to request a formal approval/rejection (default: `false`)
**get_comments** (optional) | boolean | TRUE to allow the users to comment on the files (default: `false`)
**from** (optional) | string | ID of the user in which name the request for approval will be sent. If it is not specified, the approval will be sent from the user who owns the authentication token. If it is set, the specified user will receive an email to confirm his identity before the approval request is sent. (default: `null`)

### Response

#### Codes
Http Status | Details
----------- | ----------
204 | No Content (Ok)
403 | Not allowed

### Examples

#### Request
```
curl -X POST \
  https://{BASE_URL}/api/v2/p/feedback \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{
        "emails": ["diego@twonas.com", "miguel@twonas.com"],
        "subject": "transfer test from API",
        "message": "This is a test for Diego to test the request for feedback via API",
        "files": [11111, 222222],
        "allow_see":true,
        "get_approval": true,
        "get_comments": true,
        "from": "asd832hdEWx2"
      }'
```
