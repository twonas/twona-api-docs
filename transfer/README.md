# Transfer

- [Terminology](#terminology)
- [Transfer a file (POST)](#transfer-a-file)

## Terminology

### Transfer

**Transfer**: "Transfer" is used to share artworks or files with users in and out the system.

**transfer object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**emails** | array | Collection of emails we want to share with
**subject** | string | Short explain of the share
**message** | string | Message with all details of the share
**files** | array | Collection of files shared with users

### Request
#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/p/transfer

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
  https://{BASE_URL}/api/v2/p/transfer \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{
        "emails": ["diego@twonas.com","miguel@twonas.com"],
        "subject": "transfer test from API",
        "message": "This is a test for Diego to test create a transfer from API",
        "files": [11111],
      }'
```
