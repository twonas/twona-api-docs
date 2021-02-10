# Authentication

// TODO
// If token is invalid the response will be 401 Unauthorized

## Terminology

### Organization

**Organization**: // TODO

**_organization object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | integer | Id of the organization
**name** | string | Organization name

## Context

// Explain about all data getted will be allowed

## Get a Token data (testing the API)

Return user and organization data linked with access-token.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/api/v2/session | Get session data

#### Inputs

Empty

### Response

#### Content

A _user object__ and _organization_object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK
401 | Unauthorized (access-token not valid)

### Examples

#### Request
```sh
curl -X GET \
  https://{BASE_URL}/api/v2/session \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```json
{
    "user": {
        "date_created": "2019-02-19T09:09:23+00:00",
        "email": "user@mail.com",
        "id": "iDCHaRs",
        "name": "User example name",
        "position": "User position"
    },
    "organization": {
        "id": 123122137,
        "name": "Test organization"
    }
}
```
