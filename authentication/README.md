# Authentication

Authentication is done with an API token that is sent in the header of each HTTP request under Access-token. The token is assigned per user and inherits all permissions of the user. A token can be obtained directly on Twona under Settings>APITokens.

If a token is invalid, Twona will issue a 401 Unauthorized response.

## Terminology

### Organization

**Organization**: An Organization is used to aggregate all aspects of your account. The name of the Organization will typically the name of your company :-)

**_organization object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | integer | Id (unique) of the organization
**name** | string | Name of the Organization

## Get a Token data (testing the API)

You can test the connectivity with Twona by requesting a session call which will return the user and organization data.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/api/v2/session | Get session data

#### Inputs

Empty

### Response

#### Content

A _user object__ and a _organization object_.

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
