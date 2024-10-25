# Working with Project forms

- [Terminology](#terminology)
- [Get form data (GET))](#get-form-data)
- [Submit data (POST)](#submit-data)

## Terminology

### Forms

**Form data**: Is a collection of data saved into a specific form.

**_form data object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**uuid** | string | Id (unique) of the form
**name** | string | Name of form
**fields** | array | Collection of fields

**Form data field**: The form field with data saved.

**_form data field object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**label** | string | Human readable name of field
**alias** | string | Unique name of field
**serialized_value** | any | Value of field serialized
**value** | string | Human readable value of field

### Submission response

**Submission response**: Is a response of a submit.

**_form data submit response object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**errors** | array | Collection of submit errors. Can be empty.
**submissions** | array | Collection of form data field object saved.

**_form data submit response error object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**alias** | string | Alias of failed field.
**status** | integer | Status of error (1--> not valid, 2 --> mandatory).
**message** | string | Human readable error.

_form data submission object_

## Get form data

Get the information of forms in a project.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/projects/{PROJECT_ID}/data

#### Inputs

Empty.

### Response

#### Content

A array of _form data object_.

#### Code
Http Status | Details
----------- | ----------
201 | Ok
403 | Error: Forbidden


### Examples

#### Request

```sh
curl -X GET \
  https://{BASE_URL}/api/v2/projects/1080/data \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response

```json
[
    {
        "uuid": "d28aedaf-bc45-4e25-8221-3dd8eefb135c",
        "name": "Project Information",
        "fields": [
            {
                "label": "Project Name",
                "alias": "project_name",
                "serialized_value": "",
                "value": null
            },
            {
                "label": "External Approver",
                "alias": "external_approver",
                "serialized_value": "",
                "value": null
            }
        ]
    },
    {
        "uuid": "19e2318a-a311-4306-b522-367e273ad867",
        "name": "Optional data",
        "fields": [
            {
                "label": "Some simple text field",
                "alias": "some_simple_text_field",
                "serialized_value": "some text",
                "value": "some text"
            },
            {
                "label": "Email address field",
                "alias": "email_address_field",
                "serialized_value": "som@email.com",
                "value": "som@email.com"
            },
            {
                "label": "Numbers field",
                "alias": "numbers_field",
                "serialized_value": "2222",
                "value": "2222"
            },
            {
                "label": "Url link field",
                "alias": "url_link_field",
                "serialized_value": "https://www.tuna.com",
                "value": "https://www.tuna.com"
            },
            {
                "label": "Rich text field",
                "alias": "rich_text_field",
                "serialized_value": "<p>some <strong>rich</strong> text</p>",
                "value": "some rich text"
            },
            {
                "label": "Multiple choice field",
                "alias": "multiple_choice_field",
                "serialized_value": [
                    "choice 1",
                    "choice second"
                ],
                "value": "choice 1, choice second"
            },
            {
                "label": "Date field",
                "alias": "date_field",
                "serialized_value": "2024/10/12",
                "value": "2024/10/12"
            },
            {
                "label": "Files field",
                "alias": "files_field",
                "serialized_value": [
                    {
                        "id": 311608,
                        "name": "Screenshot 2024-09-03 at 13.31.28.png",
                        "extension": "png",
                        "mimetype": "image/png",
                        "date": "2024-10-11T13:18:18+00:00",
                        "size": 35872,
                        "filesize": "35.03kB",
                        "metadata": []
                    },
                    {
                        "id": 311609,
                        "name": "Screenshot 2024-09-03 at 20.04.29.png",
                        "extension": "png",
                        "mimetype": "image/png",
                        "date": "2024-10-11T13:18:19+00:00",
                        "size": 43066,
                        "filesize": "42.06kB",
                        "metadata": []
                    }
                ],
                "value": "Screenshot 2024-09-03 at 13.31.28.png, Screenshot 2024-09-03 at 20.04.29.png"
            },
            {
                "label": "User field",
                "alias": "user_field",
                "serialized_value": {
                    "id": "KE3lf4ng",
                    "name": "Mark Fisher",
                    "email": "mark@twonas.com"
                },
                "value": "Mark Fisher"
            },
            {
                "label": "Multiple tag field",
                "alias": "multiple_tag_field",
                "serialized_value": [
                    {
                        "id": 10702,
                        "name": "Amoxicilina",
                        "group": {
                            "color": "#E87DB0"
                        }
                    },
                    {
                        "id": 10704,
                        "name": "comprimidos",
                        "group": {
                            "color": "#E87DB0"
                        }
                    }
                ],
                "value": "Amoxicilina, comprimidos"
            },
            {
                "label": "Aw link field",
                "alias": "aw_link_field",
                "serialized_value": {
                    "id": 14752,
                    "labels": [
                        {
                            "name": "3",
                            "id": 10696,
                            "group": {
                                "color": "#8CE87D",
                                "id": 320,
                                "name": "Default"
                            }
                        },
                        {
                            "name": "500 mg",
                            "id": 10703,
                            "group": {
                                "color": "#8CE87D",
                                "id": 320,
                                "name": "Default"
                            }
                        },
                        {
                            "name": "Test",
                            "id": 10691,
                            "group": {
                                "color": "#8CE87D",
                                "id": 320,
                                "name": "Default"
                            }
                        },
                        {
                            "name": "Amoxicilina",
                            "id": 10702,
                            "group": {
                                "color": "#E87DB0",
                                "id": 322,
                                "name": "Type"
                            }
                        },
                        {
                            "name": "comprimidos",
                            "id": 10704,
                            "group": {
                                "color": "#E87DB0",
                                "id": 322,
                                "name": "Type"
                            }
                        }
                    ],
                    "status": {
                        "color": "#E8D57D",
                        "name": "Pending",
                        "id": 151
                    },
                    "user_owner": {
                        "id": "KE3lf4ng",
                        "name": "Mark Fisher",
                        "email": "mark@twonas.com"
                    },
                    "date_created": "2023-02-20T10:46:42+00:00",
                    "version": "1.1"
                },
                "value": "3, 500 mg, Test, Amoxicilina, comprimidos"
            }
        ]
    }
]
```

## Submit data

Submit data in a form into a project.

### Request

#### Resource

Method | Url | Description
------- | -------- | -------
POST | https://{BASE_URL}/api/v2/projects/{PROJECT_ID}/data/{FORM_UUID} | Submit a form

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**values** | Array | Array of values

##### Value

Field name |     Type    | Description
--------- | ----------- | -----------
**alias** | string | Unique alias of a field
**value** | array, string or int | Value of field


### Response

#### Content
It returns a _form data submit response object_.

#### Code

Http Status | Details
----------- | ----------
201 | Created (saved)
406 | Not acceptable (some mandatory not satisfied or some value format is not valid)

### Examples

#### Request
```sh
curl -X POST \
  https://{BASE_URL}/api/v2/projects/1080/data/d28aedaf-bc45-4e25-8221-3dd8eefb135c \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{"values": [{
      "alias": "some_simple_text_field",
      "value": "some text"
      },{
      "alias": "email_address_field",
      "value": "sommail.com"
      }]}'
```

#### Response
```json
{
    "errors": [
        {
            "alias": "email_address_field",
            "status": 1,
            "message": "Value not valid"
        }
    ],
    "submissions": []
}
```

```sh
curl -X POST \
  https://{BASE_URL}/api/v2/projects/1080/data/d28aedaf-bc45-4e25-8221-3dd8eefb135c \
  -H 'access-token: {ACCESS_TOKEN}'
  -d '{"values": [{
      "alias": "some_simple_text_field",
      "value": "some text"
      },{
      "alias": "email_address_field",
      "value": "other@sommail.com"
      }]}'
```

#### Response
```json
{
    "errors": [],
    "submissions": [
        {
            "label": "Some simple text field",
            "alias": "some_simple_text_field",
            "serialized_value": "some text",
            "value": "some text"
        },
        {
            "label": "Email address field",
            "alias": "email_address_field",
            "serialized_value": "other@sommail.com",
            "value": "other@sommail.com"
        }
    ]
}
```