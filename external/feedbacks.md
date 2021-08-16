# Feedbacks

- [Terminology](#terminology)
- [Share a feedback (POST)](#share-an-approval)

## Terminology

### Feedback

**Feedback**: "Feedback" is used to share artworks or files with users in and out the system and ask for comments and approval or reject them.

## Request Feedback

Request for a feedback

### Request
#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/p/feedback

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**emails** (required) | array | emails which will receive the share
**subject** (required) | string | subject of the email
**message** (required) | string | text explain the matter of the share
**files** (required) | array<integer> | id files uploaded in the organization shared with the emails
**allow_see** (optional) | boolean | if the user who will receive the share can see comments from other users
**get_approval** (optional) | boolean | if the user who will receive the share can approval an artwork
**get_comments** (optional) | boolean | if the user who will receive the share can comment

### Response

#### Codes
Http Status | Details
----------- | ----------
204 | No Content

### Examples

#### Request
```
curl -X POST \
  https://{BASE_URL}/api/v2/p/feedback \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{
        "emails": ["diego@twonas.com", "miguel@twonas.com"],
        "subject": "transfer test from API",
        "message": "This is a test for Diego to test create a feedback from API",
        "files": [11111, 222222],
        "allow_see":true,
        "get_approval": true,
        "get_comments": true
      }'
```
