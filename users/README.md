# Working with users

- [Terminology](#terminology)
- [Invite a new user (POST)](#invite-a-new-user)
- [Get all invitations (GET)](#get-all-invitations)
- [Cancel an invitation (DELETE)](#cancel-an-invitation)
- [Get all users (GET)](#get-all-users)
- [Get one user (GET)](#get-one-user)
- [Update one user (PUT)](#update-one-user)
- [Deactivate one user (DELETE)](#deactivate-one-user)
- [Activate one user (POST)](#activate-one-user)
- [Add users bulk (POST)](#add-users-bulk)

## Terminology

### Invitation

**Invitation**: an invitation is the process of providing access to a new user via his own email to a specific organization.

**_invitation object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**date** | timestamp | The timestamp when the invitation was sent
**date_expiration** | timestamp | The timestamp when the invitation will expire
**email** | string | The email address to which the invitation was sent
**name** | string | The name of the invited user
**organization_id** | integer | The identifier (id) of the organization to which the user would be added
**status_code** | integer | Internal code to identify the status of the invitation: 2/3 (TODO: which codes?)
**status_text** | string | Internal text to display the status of the invitation: Expired/Pending (TODO: which texts?)
**users_group_id** | integer | The initial group assigned for the user (TODO: you send an array but get only an integer, is it the first one?)

### User

**User**: one user is a single point of access associated to a unique email address.
Each user has its own identifier, email and name among other data. The _user object_ has a structure described in the particular request.

**_user object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**date_created** | timestamp | The timestamp of the creation of the user
**email** | string | The email address of the user
**id** | string | The unique identifier (id) of the user (we call it user id)
**name** | string | The name of the user
**position** | string | The position of the user

## Invite a new user

This call will send an invitation to the user's email address provided in the parameter's set.
Once the link in the email has been clicked, the user will be added to the provided user's group.

### Request

#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/users/invitations

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**name** (required) | string | The name of the new user to invite
**email** (required) | string | The email address of the user to invite
**groups** (optional) | array | IDs (numerical) of the user groups where the user will be placed on. Groups are mostly used to limit access and grant special permissions.
**message** (optional) | string | Text to be included in the email with the invitation

### Response

#### Content

Empty

#### Code
Http Status | Details
----------- | ----------
204 | No content
422 | Error: The required fields were not found (or incomplete) or the groups (ids) are not valid

### Examples

#### Request

```
curl -X POST \
  https://{BASE_URL}/api/v2/users/invitations \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "name": "User's name",
        "email": "example@twonas.com",
        "groups": [12,14],
        "message": "Wellcome!" }'
```

## Get all invitations

Return the list of all pending or expired invitations.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/users/invitations

#### Inputs

Empty

### Response

#### Content

A collection of _invitation object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```
curl -X GET \
  https://{BASE_URL}/api/v2/users/invitations \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response

```
[
    {
        "date": "2019-06-28T10:27:21+0200",
        "email": "test1@twonas.com",
        "hash": "98dadeas37faa5",
        "name": "test1 name",
        "organization_id": 1,
        "status_code": 2,
        "status_text": "Expired",
        "users_group_id": 12
    },
    {
        "date": "2019-06-18T09:48:33+0200",
        "email": "test2@twonas.com",
        "hash": "f7c92315b32e",
        "name": "test2 name",
        "organization_id": 1,
        "status_code": 2,
        "status_text": "Expired",
        "users_group_id": 12
    }
]
```

## Cancel an invitation

Cancel a "Pending" invitation. Expired or accepted invitations cannot be canceled.

### Request

#### Resource

Method | Url
------- | --------
DELETE | https://{BASE_URL}/api/v2/users/invitations/{EMAIL}

#### Inputs

Empty

### Response

#### Content

Empty

#### Code

Http Status | Details
----------- | ----------
204 | OK

### Examples

#### Request
```
curl -X DELETE \
  https://{BASE_URL}/api/v2/users/invitations/example@twonas.com \
  -H 'access-token: {ACCESS_TOKEN}'
```
## Get all users

Return the list of all active/inactive users in the organization.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/api/v2/users | Get all active users
GET | https://{BASE_URL}/api/v2/users?deleted=true | Get all inactive users

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**deleted** (optional)  | boolean | Return all inactive users
**q** (optional) | string | Filter the users to be returned with matching conditions ([details](#search-queries))

#### Filtering list

To filter the list of users, you can add a conditional query `q` as field:query (#search-queries).

Allowed filters:

Field | Type | Description
------ | ---------- | ----------
**email**  | string | Email of the user
**name**   | string | Name of the user


### Response

#### Content
A collection of _user object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```
curl -X GET \
  https://{BASE_URL}/api/v2/users \
  -H 'access-token: {ACCESS_TOKEN}'

curl -X GET \
    https://{BASE_URL}/api/v2/users?deleted=true \
    -H 'access-token: {ACCESS_TOKEN}'
```

##### Filtering: All users with email starting with "diego"
```
curl -G \
  https://{BASE_URL}/api/v2/users \
  -H 'access-token: {ACCESS_TOKEN}'
  --data-urlencode 'q=email:diego*'
```

#### Response
```
[
    {
      "date_created": "2019-07-01T10:04:33+00:00",
      "email": "mail1@twonas.com",
      "id": "vfCssaCP",
      "name": "Mail 1 Name",
      "position": null
    },
    {
      "date_created": "2018-07-18T16:16:50+00:00",
      "email": "mail2@twonas.com",
      "id": "sjXaozdf",
      "name": "Mail2 Name",
      "position": null
    }
]
```

## Get one user

Return the details of a specific user (by its unique user id)

### Request

#### Resource

Method | Url | Description
------- | -------- |
GET | https://{BASE_URL}/api/v2/users/{USER_ID}

#### Inputs

Empty

### Response

#### Content

A _user object_.

#### Codes

Http Status | Details
----------- | ----------
200 | Ok
404 | Failed, user not found

### Examples

#### Request

```
curl -X GET \
  https://{BASE_URL}/api/v2/users/{USER_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```
{
    "date_created": "2019-06-20T10:37:21+00:00",
    "email": "user@mail.com",
    "id": "iDCHaRs",
    "name": "New user's name",
    "position": "User position"
}
```

## Update one user

Update the personal data of a specific user (by its unique user id)

### Request

#### Resource

Method | Url
------- | --------
PUT | https://{BASE_URL}/api/v2/users/{USER_ID}

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**name** (optional) | string | The NEW name of the user
**telephone** (optional) | string | The NEW phone number of the user
**position** (optional) | string | The NEW position of the user

### Response

#### Content

The updated _user object_.

#### Code

Http Status | Details
----------- | ----------
201 | Ok
404 | Failed, user not found

### Example

#### Request

```
curl -X PUT \
  https://{BASE_URL}/api/v2/users/{USER_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "name": "New user's name",
        "telephone": "+34000000000",
        "position": "User position" }'
```

#### Response
```
{
    "date_created": "2019-06-20T10:37:21+00:00",
    "email": "user@mail.com",
    "id": "iDCHaRs",
    "name": "New user's name",
    "position": "User position"
}
```

## Deactivate one user

Deactivate (make inactive) a specific a specific user (by its unique user id). Once a user is deactivated, he will no longer be able to log in.

### Resource

#### Request

Method | Url
------- | --------
DELETE | https://{BASE_URL}/api/v2/users/{USER_ID}

#### Inputs

Empty

### Response

#### Content

Empty

#### Codes

Http Status | Details
----------- | ----------
204 | No content

### Examples

#### Request

```
curl -X DELETE \
  https://{BASE_URL}/api/v2/users/{USER_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```


## Activate one user

Activate (make active) a specific a specific user (by its unique user id). Once a user is activated, he will be able to log in.

### Request

#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/users

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**id** (required) | string | The user id

### Response

#### Content

The updated _user object_.

#### Codes
Http Status | Details
----------- | ----------
201 | ok

### Examples

#### Request

```
curl -X POST \
  https://{BASE_URL}/api/v2/users \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "id": "iDCHaRs" }'
```

#### Response
```
{
    "date_created": "2019-06-20T10:37:21+00:00",
    "email": "user@mail.com",
    "id": "iDCHaRs",
    "name": "user's name",
    "position": "User position"
}
```

## Add users in bulk

Add users in bulk by WITHOUT email confirmation. Users will be added (and made active) without confirmation of the email address. A maximum of 100 users can be added per request.

### Request

#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/users/bulk

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**emails** (required) | array | Emails of the users to add
**groups** (optional) | array | Group ids to which each user will be added (maximum one group per user) (TODO: one group per user or one array per user?)

### Response

#### Content

A collection of _user object_ with the successfully added users.

#### Codes

Http Status | Details
----------- | ----------
201 | ok
400 | Emails are required
403 | This action is not allowed
413 | Too many emails
422 | One or more group ids do not exist or are invalid
422 | Error processing the collection of groups

### Examples

#### Request

```
curl -X POST \
  https://{BASE_URL}/api/v2/users/bulk \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "emails": ["newUserMail@yourDomain.com","anotherUserMail@yourDomain.com",...],
        "groups": [11,12,13,...]}'
```

#### Response

```
[
  {
    "date_created": "2019-06-20T10:37:21+00:00",
    "email": "user@mail.com",
    "id": "iDCHaRs",
    "name": "user's name",
    "position": "User position"
  }
]
```
