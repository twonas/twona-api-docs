# Working with Tags

- [Terminology](#terminology)
- [Get all Tags (GET)](#get-all-tags)
- [Get a Tag (GET)](#get-a-tag)
- [Get all Categories (GET)](#get-all-categories)
- [Create a Category (POST)](#create-a-category)
- [Create a Tag (POST)](#create-a-tag)

## Terminology

### Tag

**Tag**: A Tag is a...

**_tag object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | integer | Id (unique) of the Request
**name** | string | Tag name

**Extended* **_tag extended object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | integer | Id (unique) of the Tag
**name** | string | Tag name
**category** | Object | _category object_

### Category

**Category**: The Category represents a group of tags.

**_category object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Id (unique) of the Category
**name** | string | Category name 
**color** | string | HEX Color

## Get a Tag

Get the information of a specific tag.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/tags/{TAG_ID}

#### Inputs

Empty.

### Response

#### Content

An extended _tag object_.

#### Code
Http Status | Details
----------- | ----------
201 | Ok
404 | Error: Not found


### Examples

#### Request

```sh
curl -X GET \
  https://{BASE_URL}/api/v2/tags/{TAG_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response

```json
{
    "id": 10001,
    "name": "Tag name",
    "category": {
        "id": 101,
        "name": "Default",
        "color": "#8CE87D"
    }
}
```

## Get all tags

Return the list of all tags in the organization.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/api/v2/tags | Get all tags

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**category** (optional) | Integer | Id (unique) of the category


### Response

#### Content
A collection of _tag object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```sh
curl -X GET \
  https://{BASE_URL}/api/v2/tags \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```json
[
    {
        "id": 1001,
        "name": "Tag 1"
    },
    {
        "id": 1002,
        "name": "Tag 2"
    },
    {
        "id": 1003,
        "name": "Tag 3"
    },
    {
        "id": 1004,
        "name": "Tag 4"
    }
]
```

## Get all categories

Return the list of all tags categorie in organization.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/api/v2/tags/categories | Get all tags categories

#### Inputs

Empty.


### Response

#### Content
A collection of _category object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```sh
curl -X GET \
  https://{BASE_URL}/api/v2/tags/categories \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```json
[
    {
        "id": 101,
        "name": "Category 1",
        "color": "#8CE87D"
    },
    {
        "id": 102,
        "name": "Category 2",
        "color": "#E8A97D"
    }
]
```

## Create a Category

Create a tag category in the system.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
POST | https://{BASE_URL}/api/v2/tags/categories | Create Category

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**name** | String | Name of category
**color** | String (optional) | Color of category in hexadecimal ("#ab1cd2")


### Response

#### Content
It returns a _category object_.

#### Code

Http Status | Details
----------- | ----------
201 | Created
403 | Forbidden (The user hasn't permissio for add tags)
406 | Not acceptable (Name or color is not valid)

### Examples

#### Request
```sh
curl -X POST \
  https://{BASE_URL}/api/v2/labels/categories \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "name": "New category",
        "color": "#ab1cd2" }'
```

#### Response
```json
{
    "id": 801,
    "name": "New category",
    "color": "#ab1cd2"
}
```

## Create a Tag

Create a tag into a category in the system.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
POST | https://{BASE_URL}/api/v2/tags/categories/{CATEGORY_ID} | Create Tag

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**name** | String | Name of tag


### Response

#### Content
It returns a _tag extended object_.

#### Code

Http Status | Details
----------- | ----------
201 | Created
403 | Forbidden (The user hasn't permission for add tags)
406 | Not acceptable (Name is not valid or category is not valid)

### Examples

#### Request
```sh
curl -X POST \
  https://{BASE_URL}/api/v2/labels/categories/{CATEGORY_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "name": "New tag"}'
```

#### Response
```json
{
    "id": 8001,
    "name": "New tag",
    "category": {
        "id": 801,
        "name": "New category",
        "color": "#ab1cd2"
    }
}
```
