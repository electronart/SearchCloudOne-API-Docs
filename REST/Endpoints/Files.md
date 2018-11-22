This is the REST API Documentation for the endpoint _Files_

# Basics

* For uploading files, required content-type is `multipart/form-data`
* For all other tasks, content-type is `application/json`
* All requests must be `POST` requests.

## Authentication

A Valid API Key must be passed. 
* For Uploads, this should be sent as `APIKey` in the request header.
* For all other tasks, `APIKey` should be passed in the JSON Data.

# Uploading into an Index

* Uploading files is to be done using the Multipart/form-data protocol - [RFC7578](https://tools.ietf.org/html/rfc7578)
* All Files in the request should be sent with part-name `file`
* In addition, `APIKey` and `IndexUUID` must be passed in the header of the request.

The *APIKey* passed must be assigned *Read* Permission to the Index passed.

## Additional Fields

If the requested Index has additional fields assigned to it, field data may also be sent 
as parts in the request. Note that, if several files are sent in a request, all files will be given the same meta-data. 

To upload files with unique meta-data, make a separate request for each unique set of meta-data.

### Format
Field data should be sent with the Field name as the part name, and the field value as a string in the contents.

# Removing file(s) from an Index

Removing a file from an Index makes the file no longer searchable. The file will remain available to be restored
for 30 days, but after which it will be permanently deleted.


In order to remove file(s) from an Index, `"Action":"RemoveFilesFromIndex"` must be passed in the JSON Object.
An array of `Files` each containing a `FileUUID` should be passed, along with an `IndexUUID`.

####Example Request

```json
{
  "Action": "RemoveFilesFromIndex",
  "Files": [
    {
      "FileUUID": "########-####-####-#################"
    },
    {
      "FileUUID": "########-####-####-#################"
    }
  ],
  "IndexUUID": "########-####-####-#################"
}
```

#### Example Response

An empty success response is sent back:

`204 No Content`

## Restoring file(s) to an Index

Restoring a file to an Index makes it searchable once more

####Example Request

```json
{
  "Action": "RestoreFilesToIndex",
  "Files": [
    {
      "FileUUID": "########-####-####-#################"
    },
    {
      "FileUUID": "########-####-####-#################"
    }
  ],
  "IndexUUID": "########-####-####-#################"
}
```
#### Example Response

An empty success response is sent back:

`204 No Content`


