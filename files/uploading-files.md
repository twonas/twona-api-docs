# Uploading files

Twona uses the [TUS Open Protocol 1.0.0](https://tus.io/protocols/resumable-upload.html) to upload files. This protocol provides a mechanism for resumable file uploads via HTTP/1.1 (RFC 7230) and HTTP/2 (RFC 7540).

Files can be chunked in parts. The minimum upload part size is 5MB (except the last part). The maximum upload part size is 10MB.

## Requesting an upload resource

Files can be uploaded via an upload resource. The first step is to request one.

### Request

#### Resource

Method | Url
------- | --------
POST | https://{BASE_URL}/api/v2/files

#### Inputs

**Headers**

Header key |     Type    | Description
--------- | ----------- | -----------
**Upload-Length** (required) | integer | The size of an entire upload (in bytes).
**Upload-Metadata** (required) | string | Filename (with extension), Filetype (MIME TYPE) or other fields on Base64 encode. (Important! no spaces between fields.)

**Body**

Empty.

### Response

#### Outputs

**Headers**

Header key |     Type    | Description
--------- | ----------- | -----------
**location** | string |  URL of the upload resource

**Body**

Empty.

#### Code

Http Status | Details
----------- | ----------
201 | Created
409 | Entity already uploaded
413 | Request Entity Too Large


### Examples

#### Request
```
curl -v \
  -X POST \
  https://{BASE_URL}/api/v2/files \
  -H 'access-token: {ACCESS_TOKEN}' \
  -H 'Upload-Length: 100'
  -H 'Upload-Metadata: filename ZmlsZS5kb2M=,filetype YXBwbGljYXRpb24vbXN3b3Jk'
```

#### Response

```
< HTTP/1.1 201 Created
< Tus-Resumable: 1.0.0
< location: https://{BASE_URL}/api/v2/files/i6l028r76jsj
< Upload-Expires: Wed, 06 Nov 2019 14:24:45 GMT
...
```

## Uploading content

Once an upload URL resource is obtained, files can be uploaded. Files are uploaded using a `PATCH` request and they MUST use `Content-Type: application/offset+octet-stream`. If the file is larger than the maximum part size, you need to chunk the file in parts. The minimum part size is 5MB (except the last part). The maximum upload part size is 10MB. Once the 204 code is returned, the ID of the URL is the file ID.

### Request
#### Resource

Method | Url
------- | --------
PATCH | https://{BASE_URL}/api/v2/files/{FILE_ID}

#### Inputs

**Headers**

Header key |     Type    | Description
--------- | ----------- | -----------
**Upload-Offset** (required) | integer |  The current offset of the resource (in bytes).
**Content-Length** (required) | integer | The size of the body (in bytes).
**Content-Type**  | string | Content type of the body (MIME TYPE)

**Body**

File content.

### Response

#### Outputs

**Headers**

Header key |     Type    | Description
--------- | ----------- | -----------
**Upload-Offset** | integer | Offset of the resource (in bytes). If equal to file size, the upload process is over successfully.

**Body**

Empty.

#### Code

Http Status | Details
----------- | ----------
204 | No content
409 | Entity already uploaded
413 | Request Entity Too Large


### Examples

#### Request
```
curl -v \
  -X PATCH \
  https://{BASE_URL}/api/v2/files/i6l028r76jsj \
  -H 'access-token: {ACCESS_TOKEN}' \
  -H 'Upload-Offset: 0' \
  -H 'Content-Length: 30' \
  -H 'Content-Type: application/offset+octet-stream' \
  --data-binary @{/path/of/file}
```

#### Response
```
< HTTP/1.1 204 No content
< Tus-Resumable: 1.0.0
< Upload-Offset: 30
< Twona-File-Id: i6l028r76jsj
< Upload-Expires: Wed, 06 Nov 2019 14:24:45 GMT
...
```

## Check upload

Check the state of an upload.

### Request
#### Resource

Method | Url
------- | --------
HEAD | https://{BASE_URL}/api/v2/files/{FILE_ID}

#### Inputs

**Headers**

None.

**Body**

Empty.

### Response

#### Outputs

**Headers**

Header key |     Type    | Description
--------- | ----------- | -----------
**Upload-Offset** | integer | Offset of the resource (in bytes). If equal to file size, the upload process is over successfully.
**Upload-Length** | integer | Length of the resource registered.

**Body**

Empty.

#### Code

Http Status | Details
----------- | ----------
200 | Ok
404 | Not found
409 | Entity already uploaded


### Examples

#### Request
```
curl -v \
  -X HEAD \
  https://{BASE_URL}/api/v2/files/i6l028r76jsj \
  -H 'access-token: {ACCESS_TOKEN}' \
```

#### Response
```
< HTTP/1.1 200 OK
< Upload-Length: 100
< Upload-Offset: 30
< Twona-File-Id: i6l028r76jsj
```
