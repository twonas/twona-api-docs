# File Transfer

- [Terminology](#terminology)
- [Transfer a file (POST)](#transfer-a-file)

## Terminology

### Transfer

**Transfer**: "Transfer" is used to send files with users of the system as well as people outside of the organisation. The transfer process is done via email that contains a link to download the selected files.

**transfer object_**

WHY DEFINE THIS OBJECT?

Field name |     Type    | Description
--------- | ----------- | -----------
**emails** | array | Collection of emails to send the files to
**subject** | string | Short explanation that will be used in the email Subject
**message** | string | Long text to include further explanations or details
**files** | array | Collection of file IDs to be sent

### Request
#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/p/transfer

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**emails** (required) | array | Collection of emails to send the files to
**subject** (required) | string | Short explanation that will be used in the email Subject
**message** (required) | string | Long text to include further explanations or details
**files** (required) | array<integer> | Collection of file IDs to be sent

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
