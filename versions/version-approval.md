### Approval

**Approval**: One _approval reuqest object_ is a request to confirm an artwork is correct.

**_approval object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Unique id of the file
**user** | string | User who makes the approval
**version** | string | Version related with the artwork
**status** | string | Status of the approval (Pending, Approved, Rejected)

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

You can filter the result set by establishing filters based on the **user** of the approval or the **status** of the approval using the query method 'q=field:data'.

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
