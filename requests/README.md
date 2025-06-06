# Working with Projects (old: Requests)

- [Terminology](#terminology)
- [Get one Project (GET))](#get-a-request)
- [Get files from a Request (GET)](#get-files-attachments)
- [Get all Projects (GET)](#get-all-projects)
- [Get all Status (GET)](#get-all-status)
- [Get Status (GET)](#get-status)
- [Get Versions from Project (GET)](#get-versions) #TODO: complete the access point
- [Upload new version (POST)](#upload-new-version)
- [Create Project (POST)](#create-project)
- [Set Project (PUT)](#set-project)

## Terminology

### Project

**Project**: A Project is used to group, track and aggregate the process of creating assets (or artworks). A Project will follow the steps defined in a workflow and will link together Versions, Attachments, Messages, Approvals and anything else related to a specific project.

**_request object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | integer | Id (unique) of the Project
**labels** | array | Collection of assigned labels
**labels_text** | string | Coma separated labels as text
**info** | string | Info field (HTML text)
**date_created** | ISO 8601 date | Timestamp of creation
**priority** | float | Priority (relative to other Projects)
**status** | Status Object | Current status (step in the workflow) of the Project
**user_owner** | User Object | Creator of the Project

### Status

**Status**: The Status represents the current state of the Project within the assigned workflow.

**_status object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Id (unique) of the Status
**name** | string | Status name (the step in the workflow)
**color** | string | HEX Color

**Extended** **_status object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Id (unique) of the Status
**name** | string | Status name (the step in the workflow)
**color** | string | HEX Color
**workflow** | Object | Short _workflow object_

## Get a Project

Get the information of a specific Project.

### Project

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/v2/requests/{REQUEST_ID}

#### Inputs

Empty.

### Response

#### Content

A _request object_.

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
  https://{BASE_URL}/v2/requests/{REQUEST_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response

```json
{
    "id": 80001,
    "labels": [
        {
            "id": 33001,
            "group": {
                "color": "#E85637",
                "id": 49,
                "name": "Group 1"
            },
            "name": "Label 1"
        },
        {
            "id": 33022,
            "group": {
                "color": "#63B7AD",
                "id": 59,
                "name": "Group 2"
            },
            "name": "Label 2"
        }
    ],
    "labels_text": "Label 1, Label 2",
    "info": "Hi,<br><p>Please find attached pdf file annotated.</p><br><p>Thank you</p><p>Best regards</p><p>Charles</p>",
    "date_created": "2021-02-09T17:27:36+00:00",
    "date_updated": "2021-02-10T09:09:23+00:00",
    "priority": 80746.00001,
    "status": {
        "color": "#63B7AD",
        "id": 798,
        "name": "Finished"
    },
    "user_owner": {
        "date_created": "2019-02-19T09:09:23+00:00",
        "email": "user@mail.com",
        "id": "iDCHaRs",
        "name": "User example name",
        "position": "User position"
    },

}
```

## Get files (attachments)

Return a list of all active files (attachments) in a Project.

### Project

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/v2/requests/{REQUEST_ID}/files

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
  https://{BASE_URL}/v2/requests/{REQUEST_ID}/files \
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

## Get all Projects

Return the list of all Projects in the organization.

### Project

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/v2/requests

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**q** (optional) | string | Search query to filter results based on conditions ([details](../search/README.md#search-queries))
**sort_by** (optional) | string | Order by field ([details](../search/README.md#sort-field))
**page** | (optional) | Page of results (default: 1)

#### Filtering list

To filter the list of Projects, you can add a conditional query `q` as field:query (#search-queries).

Allowed filters:

Field | Type | Description
------ | ---------- | ----------
**status**  | integer | Id of the status
**workflow**   | integer | Id of the workflow
**user_owner**   | integer | Id of the owner
**label**   | integer | Id of the label

To order the list of Projects, you can add a sort query `sort_by` as field1,field2 (#search-queries).

Order fields:

Field | Description
------ | ----------
**priority**   | Order by
**id**   | Order by

### Response

#### Content

A collection of _request objects_.

#### Code

Http Status | Details
----------- | ----------
204 | OK

### Examples

#### Request
```sh
# Get Projects from the workflow 31 order by id desc
curl -X GET \
  https://{BASE_URL}/v2/requests \
  -H 'access-token: {ACCESS_TOKEN}' \
  --data-urlencode 'q=workflow:31' \
  --data-urlencode 'sort_by=-id'
```

#### Response
```json
[
   {
      "id":80745,
      "labels":[
         {
            "id":169,
            "group":{
               "color":"#E85637",
               "id":49,
               "name":"Manufacturer"
            },
            "name":"Label 169"
         },
         {
            "id":212,
            "group":{
               "color":"#63B7AD",
               "id":59,
               "name":"Dose"
            },
            "name":"Label 212"
         }
      ],
      "labels_text":"Label 169, Label 212",
      "info":"",
      "date_created":"2021-02-09T17:27:36+00:00",
      "date_updated":"2021-02-10T09:09:23+00:00",
      "priority":80746.00001,
      "status":{
         "color":"#63B7AD",
         "id":571,
         "name":"Finished"
      },
      "user_owner": {
          "date_created": "2019-02-19T09:09:23+00:00",
          "email": "user@mail.com",
          "id": "iDCHaRs",
          "name": "User example name",
          "position": "User position"
      },
   },
   {
      "id":80732,
      "labels":[
         {
            "id":928,
            "group":{
               "color":"#E85637",
               "id":94,
               "name":"Manufacturer"
            },
            "name":"Label 928"
         },
         {
            "id":78,
            "group":{
               "color":"#EAC739",
               "id":98,
               "name":"Presentation"
            },
            "name":"Label 78"
        }
      ],
      "labels_text":"Label 928, Label 78",
      "info":"",
      "date_created":"2021-02-09T14:17:29+00:00",
      "date_updated":"2021-02-10T11:12:37+00:00",
      "priority":80746.000005,
      "status":{
         "color":"#E1AB32",
         "id":542,
         "name":"Checking"
      },
      "user_owner": {
          "date_created": "2019-02-19T09:09:23+00:00",
          "email": "user@mail.com",
          "id": "iDCHaRs",
          "name": "User example name",
          "position": "User position"
      }
   }
]
```

## Get all status

Return the list of all Project status in the organization.

### Project

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/v2/requests/status | Get all Project status

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**workflow** (optional) | Integer | Id (unique) of the workflow


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
  https://{BASE_URL}/v2/requests/status \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```json
[
    {
        "color": "#97CBC0",
        "id": 371,
        "name": "Received"
    },
    {
       "color":"#E1AB32",
       "id":542,
       "name":"Checking"
    },
    {
       "color":"#63B7AD",
       "id":571,
       "name":"Finished"
    }
]
```

## Get status

Return the details of a Project status.

### Project

#### Resource

Method | Url | Description
------- | -------- | -------
GET | https://{BASE_URL}/v2/requests/status/{STATUS_ID} | Get request status

#### Inputs

Empty.

### Response

#### Content
An extended _status object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```sh
curl -X GET \
  https://{BASE_URL}/v2/requests/status/371 \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```json
{
    "color": "#97CBC0",
    "id": 371,
    "name": "Received",
    "workflow": {
        "id": 1,
        "name": "Default"
    }
}
```

## Upload New Version

Upload a new version from a given project.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
POST | https://{BASE_URL}/v2/requests/{id}/versions | Upload New Version

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**files** | Array | Array of files of the version
**labels** | Array | Array of labels to use
**status** | Integer | Version status ID
**version** | Integer | Version number
**subversion** | Integer | Subversion number


### Response

#### Content
It returns a _version object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```sh
curl -X POST \
  https://{BASE_URL}/v2/requests/233/versions \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "files": ["xsd332se34", "32dr33$dsde"],
        "labels": [23, 44, 456],
        "satus": 2334,
        "version": 1,
        "subversion": 1}'
```

#### Response
```json
[
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
]
```

## Create Project

Create a project in the system.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
POST | https://{BASE_URL}/v2/requests | Create Project

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**files** | Array | Array of files of the version
**labels** | Array | Array of labels to use
**workflow** | Integer | Workflow ID
**info** | String | Free text
**version** | Integer | Original version ID
**due_date** | String | String with the duedate


### Response

#### Content
It returns a _request object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```sh
curl -X POST \
  https://{BASE_URL}/v2/requests \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "files": ["xsd332se34", "32dr33$dsde"],
        "labels": [23, 44, 456],
        "workflow": 234,
        "info": "This is a new project",
        "version": 23345,
        "due_date": "2022/10/01"}'
```

#### Response
```json
[
    {
    "id": 80001,
    "labels": [
        {
            "id": 33001,
            "group": {
                "color": "#E85637",
                "id": 49,
                "name": "Group 1"
            },
            "name": "Label 1"
        },
        {
            "id": 33022,
            "group": {
                "color": "#63B7AD",
                "id": 59,
                "name": "Group 2"
            },
            "name": "Label 2"
        }
    ],
    "labels_text": "Label 1, Label 2",
    "info": "Hi,<br><p>Please find attached pdf file annotated.</p><br><p>Thank you</p><p>Best regards</p><p>Charles</p>",
    "date_created": "2021-02-09T17:27:36+00:00",
    "date_updated": "2021-02-10T09:09:23+00:00",
    "priority": 80746.00001,
    "status": {
        "color": "#63B7AD",
        "id": 798,
        "name": "Finished"
    },
    "user_owner": {
        "date_created": "2019-02-19T09:09:23+00:00",
        "email": "user@mail.com",
        "id": "iDCHaRs",
        "name": "User example name",
        "position": "User position"
    },
  }
]
```

## Set Project

Set details of a project.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
POST | https://{BASE_URL}/v2/requests | Set Project

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**request_id** | Array | ID of the project to update
**labels** (optional) | Array | Array of label IDs to use
**assigned** (optional) | String | Hash of the user to be assigned to the Project
**status** (optional) | Integer | ID of the status (the transition needs to be permitted)
**due_date** (optional) | String | String with the duedate (YYYY-MM-DD)


### Response

#### Content
It returns a _project object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```sh
curl -X POST \
  https://{BASE_URL}/v2/requests \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{ "files": ["xsd332se34", "32dr33$dsde"],
        "labels": [23, 44, 456],
        "due_date": "2022/10/01"}'
```

#### Response
```json
[
    {
    "id": 80001,
    "labels": [
        {
            "id": 33001,
            "group": {
                "color": "#E85637",
                "id": 49,
                "name": "Group 1"
            },
            "name": "Label 1"
        },
        {
            "id": 33022,
            "group": {
                "color": "#63B7AD",
                "id": 59,
                "name": "Group 2"
            },
            "name": "Label 2"
        }
    ],
    "labels_text": "Label 1, Label 2",
    "info": "Hi,<br><p>Please find attached pdf file annotated.</p><br><p>Thank you</p><p>Best regards</p><p>Charles</p>",
    "date_created": "2021-02-09T17:27:36+00:00",
    "date_updated": "2021-02-10T09:09:23+00:00",
    "priority": 80746.00001,
    "status": {
        "color": "#63B7AD",
        "id": 798,
        "name": "Finished"
    },
    "user_owner": {
        "date_created": "2019-02-19T09:09:23+00:00",
        "email": "user@mail.com",
        "id": "iDCHaRs",
        "name": "User example name",
        "position": "User position"
    },
  }
]
```

