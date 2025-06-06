# Working with users

- [Terminology](#terminology)
- [Get all workflows (GET))](#get-all-workflows)
- [Get workflow transitions (GET))](#get-workflow-transitions)

## Terminology

### Workflow

**Workflow**: A workflow defines the pathway of a request. It is a constellation of potential transitions between Request Statuses, thus a Request can only be placed in a "target" Status if there is a transition with the current Status as "origin" and the corresponding "target". A workflow typically represents a specific process in your Organization and can be an object of permission restrictions.

**_workflow object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | integer | Id of the workflow
**name** | string | Name of Workflow
**initial_status** | Status object | Initial status

### Transitions

**Transition**: A transition represents the potential to change the Status of a request from an "origin" Status to a "target" Status. Transitions a can be objects of permission restrictions

**_transition object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Id (unique) of the transition
**origin** | Status object | Origin status
**target** | Status object | Target status


## Get all workflows

Return the details of a specific request (by its unique request id)

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/v2/workflows

#### Inputs

Empty.

### Response

#### Content

A collection of _workflow object_.

#### Code
Http Status | Details
----------- | ----------
201 | Ok


### Examples

#### Request

```sh
curl -X GET \
  https://{BASE_URL}/v2/workflows \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response

```json
[
    {
        "id": 333,
        "name": "Standard",
        "initial_status": {
            "color": "#CECECE",
            "id": 81,
            "name": "Draft"
        }
    },
    {
        "id": 334,
        "name": "Design team",
        "initial_status": {
            "color": "#cecece",
            "id": 991,
            "name": "Received"
        }
    }
]
```

## Get workflow transitions

Return the list of all active transitions of a workflow.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/v2/workflows/{WORKFLOW_ID}/transitions

#### Inputs

Empty

#### Filtering

Field | Type | Description
------ | ---------- | ----------
**origin**  | integer | Id of the origin status
**target**   | integer | Id of the target status

### Response

#### Content

A collection of _transition object_.

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```sh
curl -X GET \
  https://{BASE_URL}/v2/workflows/{WORKFLOW_ID}/transitions \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d 'origin=81'
```

#### Response

```json
[
    {
        "id": 162,
        "origin": {
            "color": "#CECECE",
            "id": 81,
            "name": "Draft"
        },
        "target": {
            "color": "#EAC739",
            "id": 471,
            "name": "Processing"
        }
    },
    {
        "id": 762,
        "origin": {
            "color": "#CECECE",
            "id": 81,
            "name": "Draft"
        },
        "target": {
            "color": "#97CBC0",
            "id": 173,
            "name": "Received"
        }
    }
]
```
