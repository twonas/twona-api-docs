# Event Tracking

- [Terminology](#terminology)
- [Types and properties](#types-and-properties)
- [Get one Event (GET))](#get-a-event)
- [Get properties from an Event (GET)](#get-a-properties)
- [Get all Events (GET)](#get-all-events)

## Terminology

### Event

**EventTrack**: An EventTrack is a event was success in Twona... (bla bla bla)

**_event object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Id (unique) of the event
**date** | ISO 8601 date | Timestamp of creation
**type** | string | Type of event
**user** | User Object | User who dispatch the event

## Types and properties

All events have some properties... (bla bla bla) The event's types are in plural.

Event type |     Description    | Properties
---------- | ------------------ | -----------
**Requests** |                  |
`requests.added` | A Request added | `request` (Request Object)
`requests.labels.added` | A label (one label) is added to the Request | `request` (Request Object), `label` (Label Object)
`requests.labels.remove` | A label (one label) is removed from the Request | `request` (Request Object), `label` (Label Object)
`requests.files.added` | A file (one file) is added to the Request | `request` (Request Object), `file` (File Object)
`requests.files.removed` | A file (one file) is remove from the Request | `request` (Request Object), `file` (File Object)
`requests.status.updated` | Request Status is updated in Request | `request` (Request Object), `status` (Status Object)
`requests.messages.added` | A message is added to the Request | `request` (Request Object), `message` (Message Object)
`requests.messages.removed` | A message is removed from the Request |  `request` (Request Object), `message` (Message Object)
**Versions** |          |        
`versions.added` | A Version is added | `version` (Version Object)
`versions.labels.added` | A label (one label) is added to Version | `version` (Version Object), `label` (Label Object)
`versions.labels.removed` | A label (one label) is removed from Version | `version` (Version Object), `label` (Label Object)
`versions.files.added` | A file is added to Version | `version` (Version Object), `file` (File Object)
`versions.status.updated` | Version Status is updated in Version | `version` (Version Object), `status` (Status Object)
`versions.messages.added` | A message is added to the Version | `version` (Version Object), `message` (Message Object)
`versions.messages.removed` | A message is removed to the Version | `version` (Version Object), `message` (Message Object)

## Get a Event

Get the information of a specific event.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/events/{EVENT_ID}

#### Inputs

Empty.

### Response

#### Content

A _event object_.

#### Code
Http Status | Details
----------- | ----------
201 | Ok
404 | Error: Not found


### Examples

#### Request

```sh
curl -X GET \
  https://{BASE_URL}/api/v2/events/{EVENT_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response

```json
{
    "date": "2019-07-05T14:09:48+00:00",
    "id": "df73fbb2178875b46003346edd293ec5",
    "type": "requests.messages.added",
    "user": {
        "date_created": "2019-02-19T09:09:23+00:00",
        "email": "user@mail.com",
        "id": "iDCHaRs",
        "name": "User example name",
        "position": "User position"
    }
}
```

## Get properties

Return a event's properties.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/events/{EVENT_ID}/properties

#### Inputs

Empty

### Response

#### Content

An object depend of the event type. If the Request or Version is not allowed, the call return a forbidden.

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
  https://{BASE_URL}/api/v2/events/{EVENT_ID}/properties \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response

```json
{
    "request": {
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
    "message": {
        "text": "This the content of a message in <strong>HTML</strong>",
        "date": "2021-03-16T09:40:55+00:00",
        "files": [],
        "id": 101643,
        "user": {
            "date_created": "2019-02-19T09:09:23+00:00",
            "email": "user@mail.com",
            "id": "iDCHaRs",
            "name": "User example name",
            "position": "User position"
        }
    }
}
```

## Get all Events

Return the list of all events.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/events

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**type** (optional) | string | Property for filter result based on type of event
**user** (optional) | string  | Property for filter result based on user of event
**q** (optional) | string | Search query to filter results based on conditions ([details](../search/README.md#search-queries))
**page** | (optional) | Page of results (default: 1)

#### Filtering list

To filter the list of events you can select any property of events. You can add a conditional query `q` as field:query (#search-queries).

Allowed filters: See in types and properties table.

The order is date of event DESC.

### Response

#### Content

A collection of _event objects_.

#### Code

Http Status | Details
----------- | ----------
204 | OK

### Examples

#### Request
```sh
# Get requests from the workflow 31 order by id desc
curl -X GET \
  https://{BASE_URL}/api/v2/events \
  -H 'access-token: {ACCESS_TOKEN}' \
  --data-urlencode 'q=request:31' \
  --data-urlencode 'type=requests.status.updated'
```

#### Response
```json
[
    {
        "date": "2019-07-05T14:09:48+00:00",
        "id": "df73fbb2178875b46003346edd293ec5",
        "type": "requests.status.updated",
        "user": {
            "date_created": "2019-02-19T09:09:23+00:00",
            "email": "user@mail.com",
            "id": "iDCHaRs",
            "name": "User example name",
            "position": "User position"
        }
    },
    {
        "date": "2019-07-05T14:09:25+00:00",
        "id": "354864fff64d1d90ced37930e46c449a",
        "type": "requests.status.updated",
        "user": {
            "date_created": "2019-02-19T09:09:23+00:00",
            "email": "user@mail.com",
            "id": "iDCHaRs",
            "name": "User example name",
            "position": "User position"
        }
    },
    {
        "date": "2019-07-05T14:09:12+00:00",
        "id": "24a925bce697aa1a9f8c9bc00e343acd",
        "type": "requests.status.updated",
        "user": {
            "date_created": "2019-02-19T09:09:23+00:00",
            "email": "user@mail.com",
            "id": "iDCHaRs",
            "name": "User example name",
            "position": "User position"
        }
    }
]
```
