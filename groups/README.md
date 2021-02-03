# Working with groups

- [Add group (POST)](#add-group)
- [Get one group (GET)](#get-one-group)
- [Get all groups (GET)](#get-all-groups)
- [Update one group (PUT)](#update-one-group)
- [Deactivate one group (DELETE)](#deactivate-one-group)
- [Activate one group (POST)](#activate-one-group)
- [Get users in group (GET)](#get-users-in-group)
- [Add one user to group (POST)](#add-one-user-to-group)
- [Deactivate one user in group (DELETE)](#deactivate-one-user-in-group)

## Terminology

### Groups

**Group**: Groups are a way to organise users based on specific permissions or automation sets (also called conditions). They are virtual construct and do not impose any hard constrains on the limitations of access of a user unless the corresponding conditions are set.

Each group has a unique id and name.

**_group object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**active** | boolean | The status of the group (active = true, inactive = false)
**date_created** | timestamp | The timestap of the creation of the group
**date_updated** | timestamp | The timestamp of the last update of group
**id** | integer | The group id
**name** | string | The name of the group

## Add group

Create a new group.

### Request

#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/groups

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**name** (required) | string | The name of the group

### Response

#### Content
A _group object_.

#### Code
Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```
curl -X POST \
  https://{BASE_URL}/api/v2/groups \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{"name": "new group's name"}'
```

#### Response
```
{
    "active": true,
    "date_created": "2019-07-01T11:47:04+00:00",
    "date_updated": "2019-07-01T11:47:04+00:00",
    "id": 160,
    "name": "new group's name"
}
```

## Get one group

Get the information of a specific group.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/groups/{GROUP_ID}


### Response

#### Content
A  _group object_.

#### Codes
Http Status | Details
----------- | ----------
200 | OK
404 | Group not found

### Examples

#### Request
```
curl -X GET \
  https://{BASE_URL}/api/v2/groups/{GROUP_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```
{
    "active": true,
    "date_created": "2019-07-01T11:43:04+00:00",
    "date_updated": "2019-07-01T11:43:04+00:00",
    "id": 111,
    "name": "Main group"
}
```

## Get all groups

Return a collection of all active/inactive groups.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/api/v2/groups | Get all active groups
GET | https://{BASE_URL}/api/v2/groups?deleted=true | Get all inactive groups

### Response

#### Content

A collection of _group object_.

#### Code

Http Status | Details
----------- | ----------
200 | Ok

### Examples

#### Request
```
curl -X GET \
  https://{BASE_URL}/api/v2/groups \
  -H 'access-token: {ACCESS_TOKEN}'

curl -X GET \
    https://{BASE_URL}/api/v2/groups?deleted=true \
    -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```
[
    {
        "active": true,
        "date_created": "2019-07-01T11:47:04+00:00",
        "date_updated": "2019-07-01T11:47:04+00:00",
        "id": 111,
        "name": "Main group"
    },
    {
        "active": true,
        "date_created": "2019-07-01T11:47:04+00:00",
        "date_updated": "2019-07-01T11:47:04+00:00",
        "id": 160,
        "name": "new group's name"
    }
]
```

## Update one group

Update the data of a specific group.

### Request
#### Resource

Method | Url
------- | --------
PUT | https://{BASE_URL}/api/v2/groups/{GROUP_ID}

#### Inputs

Field name | Type | Description
--------- | ----------- | -----------
**name** (required) | string | The NEW name of the group.
**active** (optional) | boolean | The NEW status of the group.

### Response

#### Content
The updated _group object_.

#### Code

Http Status | Details
----------- | ----------
201 | ok

### Examples

#### Request
```
curl -X PUT \
  https://{BASE_URL}/api/v2/groups/{GROUP_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "name": "New name",
        "active": false }'
```
#### Response
```
{
    "active": false,
    "date_created": "2019-07-01T11:47:04+00:00",
    "date_updated": "2019-07-01T11:47:04+00:00",
    "id": 160,
    "name": "New name"
}
```

## Deactivate one group

Deactivate a specific group. The users in this group will still be active but all conditions associated with the group will stop executing. Users cannot be added to a deactivated group.

### Request

#### Resource

Method | Url
------- | --------
DELETE | https://{BASE_URL}/api/v2/groups/{GROUP_ID}

### Response

#### Content

Empty

#### Code

Http Status | Details
----------- | ----------
200 | ok

### Examples

#### Request
```
curl -X DELETE \
  https://{BASE_URL}/api/v2/groups/{GROUP_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```

## Activate one group

Activate a specific group. Users can be added to an active group.

### Request

#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/groups

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**id** (required) | integer | Group id

### Response

#### Content

A _group object_.

#### Code

Http Status | Details
----------- | ----------
200 | ok

### Examples

#### Request
```
curl -X POST \
  https://{BASE_URL}/api/v2/groups \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{"id": 160}'
```

#### Response
```
{
    "active": true,
    "date_created": "2019-07-01T12:47:10+00:00",
    "date_updated": "2019-07-02T08:22:49+00:00",
    "id": 158,
    "name": "test July updated new"
}
```

## Get users in group

Return the collection of users in a specific group.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/api/v2/groups/{GROUP_ID}/users | Get users in the group with GROUP ID

### Response

#### Content

A collection of _user object_.

#### Code

Http Status | Details
----------- | ----------
200 | ok

### Examples

#### Request
```
curl -X GET \
  https://{BASE_URL}/api/v2/groups/{GROUP_ID}/users \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```
[
    {
      "date_created": "2019-07-01T10:04:33+00:00",
      "email": "mail1@twonas.com",
      "id": "vfCswDrcRsaCP",
      "name": "Mail 1 Name",
      "position": null
    },
    {
      "date_created": "2018-07-18T16:16:50+00:00",
      "email": "mail2@twonas.com",
      "id": "sjXaodaEwzdf",
      "name": "Mail2 Name",
      "position": null
    }
]
```

## Add one user to group

Add one user in a specific group.

### Request

#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/groups/{GROUP_ID}/users

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**id** (required) | string | The User id to add to the group

### Response

#### Content
A _user object_.

#### Code
Http Status | Details
----------- | ----------
201 | ok

### Examples

#### Request
```
curl -X POST \
  https://{BASE_URL}/api/v2/groups/{GROUP_ID}/users \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{"id": "iDCHaRs"}'
```
#### Response
```
{
    "date_created": "2019-06-20T10:37:21+00:00",
    "email": "user@mail.com",
    "id": "iDCHaRs",
    "name": "User name",
    "position": "User position"
}
```

## Deactivate one user in group

Dactivate one user in a specific group. The user will no longer be part of set of active users in a group.

### Request

#### Resource

Method | Url
------- | --------
DELETE | https://{BASE_URL}/api/v2/groups/{GROUP_ID}/users/{USER_ID}

### Response

#### Content

Empty

#### Code
Http Status | Details
----------- | ----------
204 | No content

### Examples

#### Request
```
curl -X DELETE \
  https://{BASE_URL}/api/v2/groups/{GROUP_ID}/users/{USER_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```
