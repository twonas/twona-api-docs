# Event Tracking

- [Terminology](#terminology)
- [Types and properties](#event-types-and-properties)
- [Get one Event (GET))](#get-an-event)
- [Get properties from an Event (GET)](#get-properties)
- [Get all Events (GET)](#get-all-events)

## Terminology

### Event

An event is an action that has occured in your account. This can be trigged by a user action or as a result of an automation rule. In the case of an event triggered by an automation rule, the user that triggered the action that triggered the rule is considered as the user that triggered the event. 

**_event object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Id (unique) of the event
**date** | ISO 8601 date | Timestamp of the logging of the event (can differ slightly from timesampt of the action)
**type** | string | Type of event
**user** | User Object | User that triggered the event

## Event Types and Properties

Below you can find the list of supported events along with their properties. The properties of an event are a collection of objects related to the event.

Event type |     Description    | Properties
---------- | ------------------ | -----------
**Requests** |                  |
`requests.added` | A Request was added | `request` (Request Object)
`requests.labels.added` | A label (one label) is added to a Request | `request` (Request Object), `label` (Label Object)
`requests.labels.remove` | A label (one label) is removed from a Request | `request` (Request Object), `label` (Label Object)
`requests.files.added` | A file (one attachment) is added to a Request | `request` (Request Object), `file` (File Object)
`requests.files.removed` | A file (one attachment) is deactivated in a Request | `request` (Request Object), `file` (File Object)
`requests.status.updated` | The Status of a Request is changed | `request` (Request Object), `status` (Status Object)
`requests.messages.added` | A message is left in a Request | `request` (Request Object), `message` (Message Object)
`requests.messages.removed` | A message is deactivated from a Request |  `request` (Request Object), `message` (Message Object)
**Versions** |          |        
`versions.added` | A Version is added | `version` (Version Object)
`versions.labels.added` | A label (one label) is added to a Version | `version` (Version Object), `label` (Label Object)
`versions.labels.removed` | A label (one label) is removed from a Version | `version` (Version Object), `label` (Label Object)
`versions.files.added` | A file is added to a Version | `version` (Version Object), `file` (File Object)
`versions.status.updated` | The Status of a Version is changed | `version` (Version Object), `status` (Status Object)
`versions.messages.added` | A message is left in a Version | `version` (Version Object), `message` (Message Object)
`versions.messages.removed` | A message is deactivated from a Version | `version` (Version Object), `message` (Message Object)

## Get an Event

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

An _event object_.

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

Get the properties of an event.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/events/{EVENT_ID}/properties

#### Inputs

Empty

### Response

#### Content

The content of the event will be a collection of objects related to the event. If any of the related objects cannot be accesed due to permissions, a Forbidden error will be returned.

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

Get all the events in the organization.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/events

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**type** (optional) | string | Type of the event to be used as a filter
**user** (optional) | string  | User that triggered the event to be used as filter
**q** (optional) | string | Search query to filter results based on conditions ([details](../search/README.md#search-queries))
**page** | (optional) | Page of results (default: 1)

#### Filtering list

You can also filter by any property of the event with a conditional query `q` as field:query ([details](../search/README.md#search-queries)).

Allowed filters: See in types and properties table.

The list of events is ordered by date DESC.

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
