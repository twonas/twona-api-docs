# Feedback

- [Terminology](#terminology)
- [Share a file (POST)](#share-a-file)
- [Share an approval (POST)](#share-an-approval)
- [Get all approvals by version (GET)](#get-approvals)

## Terminology

### Share

**Share**: "Share" is used to share information or ask for a feedback with users in and out the system. We can share files (**_share transfer_**) or ask for feedback (**_share feedback_**). Those _share objects_ have a main structure.

**_share object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**type** | string | type of share (transfer | feedback)
**emails** | array | Collection of emails we want to share with
**subject** | string | Short explain of the share
**message** | string | Message with all details of the share
**files** | array | Collection of files shared with users

## Share a file (Transfer)

Create a new share with one user or email.

### Request
#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/p/share_transfer

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**emails** (required) | array | emails which will receive the share
**subject** (required) | string | subject of the email
**message** (required) | string | text explain the matter of the share
**files** (required) | array | if files shared with the emails

### Response

#### Code
Http Status | Details
----------- | ----------
204 | No Content

### Examples

#### Request
```
curl -X POST \
  https://{BASE_URL}/api/v2/p/share_transfer \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{
        "type": "transfer",
        "emails": ["diego@twonas.com","miguel@twonas.com"],
        "subject": "transfer test from API",
        "message": "This is a test for Diego to test create a transfer from API",
        "files": [11111],
      }'
```

## Request Feedback

Request for a feedback

### Request
#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/p/share_feedback

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**emails** (required) | array | emails which will receive the share
**subject** (required) | string | subject of the email
**message** (required) | string | text explain the matter of the share
**files** (required) | array | if files shared with the emails
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
curl -X GET \
  https://{BASE_URL}/api/v2/files/{FILE_ID} \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{
        "type": "transfer",
        "emails": ["diego@twonas.com","miguel@twonas.com"],
        "subject": "transfer test from API",
        "message": "This is a test for Diego to test create a transfer from API",
        "files": [11111],
        "allow_see":true,
        "get_approval": true,
        "get_comments": true
      }'
```
