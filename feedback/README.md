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

### Approval

**Approval**: One _approval reuqest object_ is a request to confirm an artwork is correct.

**_approval object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Unique id of the file
**user** | string | User who makes the approval
**version** | string | Version related with the artwork
**status** | string | Status of the approval (Pending, Approved, Rejected)

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
**type** (required) | string | Share type
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
**type** (required) | string | Share type
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

## Get Approvals

Return all the approvals .

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/versions/{VERSION_ID}/approvals

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**q** (optional) | string | Search query to filter results based on conditions ([details](../search/README.md#search-queries))

#### Filtering list

You can filter the result set by establishing filters based on the user of the approval or the status of the approval using the query method 'q=field:data'.

### Response
#### Content

A collection of _approval object_

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```
curl -X GET \
  https://{BASE_URL}/api/v2/versions/VERSION_ID/approvals \
  -H 'access-token: {ACCESS_TOKEN}'
```

```
# Filtering: All the approvals shared with the users who start with "rafael"

curl -G \
  https://{BASE_URL}/api/v2/versions/VERSION_ID/approvals \
  -H 'access-token: {ACCESS_TOKEN}'
  --data-urlencode '?q=user:rafael*'
```

#### Response

```
[
    {
        "id": 1,
        "version": {
            "id": 14283,
            "version": "1.0",
            "labels": [
                {
                    "id": 7825,
                    "group": {
                        "active": true,
                        "color": "",
                        "id": 83,
                        "name": "PROJECT"
                    },
                    "name": "TEST"
                },
                {
                    "id": 1193,
                    "group": {
                        "active": true,
                        "color": "",
                        "id": 86,
                        "name": "CUSTOMER"
                    },
                    "name": "Sandoz"
                }
            ],
            "labels_text": "TEST, NEW",
            "version_number": 1,
            "subversion_number": 0,
            "date_created": "2021-02-24T12:41:41+00:00",
            "user_owner": {
                "date_created": "2018-06-27T17:30:26+00:00",
                "email": "diego@twonas.com",
                "id": "rmohnmxz",
                "name": "Diego Perez",
                "position": "Master"
            },
            "status": {
                "active": true,
                "color": "#97CBC0",
                "id": 14,
                "name": "Current"
            }
        },
        "user": {
            "date_created": "2018-06-27T17:30:26+00:00",
            "email": "rafael@twonas.com",
            "id": "awdvbhtl",
            "name": "Rafa",
            "position": null
        },
        "date_created": "2021-05-04T08:13:06+0000",
        "active": true,
        "status_tracks": [
            {
                "date": "2021-05-04T08:13:06+00:00",
                "id": 1,
                "message": null,
                "status_code": 0,
                "status_text": "Pending"
            }
        ]
    }
]
```
