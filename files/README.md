# Working with files

- [Terminology](#terminology)
- [Get a file (GET)](#get-a-file)
- [Download a file (GET)](#download-a-file)
- [Add metadata to File (PUT)](#add-metadata-to-file)
- [Get all organization's files (GET)](#get-all-files)

- **Uploading files**
    - [Request upload a file (POST)](uploading-files.md#requesting-an-upload-resource)
    - [Uploading content (PATCH)](uploading-files.md#uploading-content)
    - [Check upload (HEAD)](uploading-files.md#check-upload)

## Terminology

### Files

**File**: One _file object_ is a file uploaded to the system by a user with more information about it. This _file object_ is temporal until it's used in some process or some attribute is updated. The _file object_ has a structure described in the particular request.

**_file object_**

Field name |     Type    | Description
--------- | ----------- | -----------
**id** | string | Unique id of the file
**name** | string | Filename with extension
**extension** | string | Extension of the file
**filesize** | string | File size in human readable format
**size** | int | File size (in bytes)
**date** | timestamp | Timestamp of the creation of the file
**metadata** | object | If present, metadata associated with the file (added by the user)

## Get a file

Obtain details of a specific file.

### Request
#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/files/{FILE_ID}

### Response

#### Content
A _file object_.

#### Codes
Http Status | Details
----------- | ----------
200 | Ok
404 | Failed, file not found

### Examples

#### Request
```
curl -X GET \
  https://{BASE_URL}/api/v2/files/{FILE_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
```

#### Response
```
{
    "date":"2019-0702T12:24:59+0200",
    "extension":"png",
    "id":"e40ed4433ba4",
    "filesize":"2.96MB",
    "name":"imagen.png",
    "size":3108445
}
```

## Download a file

Download a specific file.

### Request
#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/files/{FILE_ID}

**Headers**

Header key |     Type    | Description
--------- | ----------- | -----------
**Content-Type**  | string | Content type of the body


### Response

#### Codes
Http Status | Details
----------- | ----------
200 | Ok
404 | Failed, file not found

### Examples

#### Request
```
curl -X GET \
  https://{BASE_URL}/api/v2/files/{FILE_ID} \
  -H 'access-token: {ACCESS_TOKEN}'
  -H 'Content-Type: application/offset+octet-stream' \
```

## Add metadata to File

Add custom metadata to a specific file. When metadata is added to a file, the file becomes a persistent file and is no longer a temporal file. If the file was temporal, a new reference to the persistent file is returned.

### Request
#### Resource

Method | Url
------- | --------
PUT | https://{BASE_URL}/api/v2/files/{FILE_ID}

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**metadata** (required) | JSON | Keys and values of the metadata to be inserted

### Response
#### Content

A _file object_.

#### Code

Http Status | Details
----------- | ----------
301 | Redirect to new file updated
200 | Metadata successfully added to file

### Examples

#### Request
```
curl \
  https://{BASE_URL}/api/v2/files \
  -H 'access-token: {ACCESS_TOKEN}' \
  -d '{"metadata":{"key":"value","key2":"value2"}}'
```

### Response

If the file id is a temporal file, the response will redirect to the new file.
If the file id is a persisted file, the response will be a _file Object_

## Get all files

Return all the files from the organization.

### Request

#### Resource

Method | Url
------- | --------
GET | https://{BASE_URL}/api/v2/files

#### Inputs

Field name |     Type    | Description
--------- | ----------- | -----------
**q** (optional) | string | Search query to filter results based on conditions ([details](#search-queries))

#### Filtering list

You can filter the result set by establishing filters based on the metadata fields using the query method 'q=field:data'.

### Response
#### Content

A collection of _file object_

#### Code

Http Status | Details
----------- | ----------
200 | OK

### Examples

#### Request
```
curl -X GET \
  https://{BASE_URL}/api/v2/files \
  -H 'access-token: {ACCESS_TOKEN}'
```

```
# Filtering: All files with a field called owner start with "diego"

curl -G \
  https://{BASE_URL}/api/v2/files \
  -H 'access-token: {ACCESS_TOKEN}'
  --data-urlencode 'q=owner:diego*'
```

#### Response

```
[
    {
        "active": true,
         "date": "2019-10-30T10:02:43+00:00",
         "extension": "pdf",
         "filesize": "18.65kB",
         "id": 267719,
         "mimetype": "application/pdf",
         "name": "ref.pdf",
         "size": 19100,
         "metadata": {
             "updated": 20191108,
             "owner": "diego",
             "tag": "test"
         }
    },
    {
        "active": true,
         "date": "2019-10-30T10:02:43+00:00",
         "extension": "pdf",
         "filesize": "18.65kB",
         "id": 267719,
         "mimetype": "application/pdf",
         "name": "ref.pdf",
         "size": 19100,
         "metadata": {
             "updated": 20191011,
             "owner": "diego",
             "tag": "test2"
         }
    }
]
```
