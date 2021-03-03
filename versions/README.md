# Working with Versions

- [Terminology](#terminology)
- [Get one Version (GET))](#get-one-version)
- [Get files from a Version (GET)](#get-files-from-a-version)
- [Get all Versions (GET)](#get-all-versions)
- [Get all Statuses (GET)](#get-all-status)

## Terminology

### Version

**Version**: A version is a collection of files that are considered a set to which a specific state and version number (a human one and a computer one) is assigned. Versions are used to ensure the correct files are used. A version is identified by a unique ID and a user number.number model (ex. Version 1.2) and is defined by the set of labels assigned to it. A version must contain AT LEAST one file.

**_version object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | integer | Id of the Version
**labels** | array | Collection of labels
**labels_text** | string | Coma separated labels
**version_number** | integer | Version number
**subversion_number** | integer | Subversion number
**version** | string | String format Version dot Subversion
**date_created** | ISO 8601 date | The timestamp of the creation of the version
**status** | Status Object | Current status of the Version (THIS IS WRONG)
**user_owner** | User Object | User who uploaded the version

### Status

**Status**: A version can be set to a specific Status (ex. Approved). This determines the purpose of the Version and can be used to filter, limit access, etc.

**_status object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Id (unique) of the status
**name** | string | The name of the status
**color** | string | HEX Color 

## Get one Version

Return the details of a specific version.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/versions/{VERSION_ID}

#### Inputs

Empty.

### Response

#### Content

A _version object_.

#### Code
Http Status | Details
----------- | ----------
201 | Ok
403 | Error: Forbidden
404 | Error: Not found


### Examples

#### Request

```sh
curl -X GET \
  https://{BASE_URL}/api/v2/versions/{VERSION_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response

```json
{
    "id": 956783,
    "labels": [
        {
            "id": 928,
            "group": {
                "color": "#E85637",
                "id": 49,
                "name": "Manufacturer"
            },
            "name": "Label 928"
        },
        {
            "id": 78,
            "group": {
                "color": "#EAC739",
                "id": 89,
                "name": "Presentation"
            },
            "name": "Label 78"
        }
    ],
    "labels_text": "Label 928, Label 78",
    "version": "4.2",
    "version_number": 4,
    "subversion_number": 2,
    "time_created": "2021-02-10T11:32:07+00:00",
    "user_owner": {
        "date_created": "2019-02-19T09:09:23+00:00",
        "email": "user@mail.com",
        "id": "iDCHaRs",
        "name": "User example name",
        "position": "User position"
    },
    "status": {
        "color": "#E1AB32",
        "id": 12,
        "name": "In process"
    }
}
```

## Get files from a Version

Return the list of all files in a version.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/versions/{VERSION_ID}/files

#### Inputs

Empty

### Response

#### Content

A collection of _files object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK
403 | Error: Forbidden
404 | Error: Not found

### Examples

#### Request
```sh
curl -X GET \
  https://{BASE_URL}/api/v2/versions/{VERSION_ID}/files \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response

```json
[
    {
        "active": true,
        "date": "2021-02-09T17:27:36+00:00",
        "extension": "pdf",
        "filesize": "274.59kB",
        "id": 1844020,
        "mimetype": "application/pdf",
        "name": "file1.pdf",
        "size": 281183,
        "metadata": []
    },
    {
        "active": true,
        "date": "2021-02-09T17:27:36+00:00",
        "extension": "pdf",
        "filesize": "154.56kB",
        "id": 1844020,
        "mimetype": "application/pdf",
        "name": "file2.pdf",
        "size": 154560,
        "metadata": []
    }
]
```

## Get all Versions

Return the list of all versions in the organization.

### Version

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/versions

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**q** (optional) | string | Search query to filter results based on conditions ([details](../search/README.md#search-queries))
**sort_by** (optional) | string | Order by field ([details](../search/README.md#sort-field))
**page** | (optional) | Page of results (default: 1)

#### Filtering list

To filter the list of versions, you can add a conditional query `q` as field:query (#search-queries).

Allowed filters:

Field | Type | Description
------ | ---------- | ----------
**status**  | integer | Id of the status
**label**   | integer | Id of the label

To order the list of versions, you can add a short query `short_by` as field1,field2 (#search-queries).

Order fields:

Field | Description
------ | ----------
**id**   | Order by

### Response

#### Content

A collection of _version objects_.

#### Code

Http Status | Details
----------- | ----------
204 | OK

### Examples

#### Request
```sh
# Get requests from the workflow 31 order by id desc
curl -X GET \
  https://{BASE_URL}/api/v2/versions \
  -H 'access-token: {ACCESS_TOKEN}' \
  --data-urlencode 'q=status:12' \
  --data-urlencode 'sort_by=-id'
```

#### Response
```json
[
    {
        "id": 956783,
        "labels": [
            {
                "id": 922,
                "group": {
                    "color": "#E85637",
                    "id": 49,
                    "name": "Manufacturer"
                },
                "name": "Label 922"
            },
            {
                "id": 78,
                "group": {
                    "color": "#EAC739",
                    "id": 89,
                    "name": "Presentation"
                },
                "name": "Label 78"
            }
        ],
        "labels_text": "Label 922, Label 78",
        "version": "1.2",
        "version_number": 1,
        "subversion_number": 2,
        "time_created": "2021-02-10T11:32:07+00:00",
        "user_owner": {
            "date_created": "2019-02-19T09:09:23+00:00",
            "email": "user@mail.com",
            "id": "iDCHaRs",
            "name": "User example name",
            "position": "User position"
        },
        "status": {
            "color": "#E1AB32",
            "id": 12,
            "name": "In process"
        }
    },
    {
        "id": 956743,
        "labels": [
            {
                "id": 923,
                "group": {
                    "color": "#E85637",
                    "id": 49,
                    "name": "Manufacturer"
                },
                "name": "Label 923"
            },
            {
                "id": 76,
                "group": {
                    "color": "#EAC739",
                    "id": 89,
                    "name": "Presentation"
                },
                "name": "Label 76"
            }
        ],
        "labels_text": "Label 923, Label 76",
        "version": "4.2",
        "version_number": 4,
        "subversion_number": 2,
        "time_created": "2021-02-09T11:32:07+00:00",
        "user_owner": {
            "date_created": "2019-02-19T09:09:23+00:00",
            "email": "user@mail.com",
            "id": "iDCHaRs",
            "name": "User example name",
            "position": "User position"
        },
        "status": {
            "color": "#E1AB32",
            "id": 12,
            "name": "In process"
        }
    }
]
```

## Get all status

Return the list of all version status in the organization.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/api/v2/versions/status | Get all versions status

#### Inputs

Empty

### Response

#### Content
A collection of _status object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```sh
curl -X GET \
  https://{BASE_URL}/api/v2/versions/status \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```json
[
    {
        "color": "#E1AB32",
        "id": 12,
        "name": "In process"
    },
    {
        "color": "#E85637",
        "id": 267,
        "name": "Discarded Version"
    },
    {
       "color":"#63B7AD",
       "id":571,
       "name":"Finished"
    }
]
```
