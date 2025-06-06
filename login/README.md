# Auto-key for Login

Auto-key is a method based on a "magic link" that allows to log a user without password. This is the prefered method for integrating within a third party application so that user control is made through the API and transparent for the users.

**You need additional permissions for this feature. If you you obtain an error when making this API calls, please contact our customer support.**

## Request a magic link

Request a link to log in a specific user.

Twona will return an _Autokey_ to which you can append the access URL with an `href` query and encoding for url `urlencode` the section you want the user to access.

An example in PHP:

```php
$urlToApp = $autologin."?href=".urlencode("https://app.twonas.com/app/p/apollo/upload?reference=".$referenceId."&target=".$targetId);
```

### Request

#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/v2/login/auto-key

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**email** (required) | string | Email of the user you wish to log in

### Response

#### Response headers
Header key |     Type    | Description
--------- | ----------- | -----------
**Location** | string | Magic link.

#### Body content

Empty.

#### Code

Http Status | Details
----------- | ----------
204 | Ok
401 | Unauthorized (you do not have permission to request an auto-key)
403 | Not allowed (the user does not exist)

### Examples

#### Request
```
curl -X POST \
  https://{BASE_URL}/v2/login/auto-key \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{"email":"user@twonas.com"}'
```

#### Response
```
< HTTP/1.1 204 No Content
< Location: https://{URL_BASE}/login/auto-key/{HASH}/{CHECKSUM}
```
