This documentation is currently private as we don't have a mechanism to issue API Keys with the permissions needed to
manage indexes to customers yet.

This is the REST API Documentation for the endpoint _Indexes_ 

# Basics

* Accepts content-type `application/json` only.
* Accepts `POST` requests only.

# Authentication

A valid API Key must be passed to IndexManager within the JSON data.

# Actions

The _IndexManager_ expects one of the following actions:-

* `ListIndexes`
* `CreateIndex`
* `DeleteIndex`
* `AppendMetadataSpecToIndex`

## Action `ListIndexes`

Lists the Indexes that belong to the account.
No additional arguments need to be passed.

### Example Request

```json
{
   "APIKey": "ba020ccd-80bc-463c-8b1d-984544c1478a",
   "action": "list-index"
}
```

### Example Response

```json
{
  "Indexes": [
    {
      "IndexUUID": "6fea98a8-baac-4849-a2dd-e4ca0625a500",
      "IndexID": 12,
      "Name": "Cases",
      "Description": "Ongoing Legal Cases",
      "Size": 4096,
      "FileCount": 1728,
      "Created": "2018-06-06T15:10:46.380Z",
      "Modified": "2018-06-06T15:10:46.381Z",
      "RecycleCount": 0
    },
    {
      "IndexUUID": "6fea98a8-baac-4849-a2dd-e4ca0625a500",
      "IndexID": 15,
      "Name": "Cases",
      "Description": "Ongoing Legal Cases",
      "Size": 6549875324,
      "FileCount": 567901,
      "Created": "2018-06-06T15:10:46.381Z",
      "Modified": "2018-06-06T15:10:46.381Z",
      "RecycleCount": 5
    }
  ]
}
```

## Action `CreateIndex`

Creates a new Index

Expects `Name`, `Description` and `Tech` fields to be passed.
* _description_ may be an empty string
* _tech_ refers to the desired Index Technology Code. Refer to [Index Technologies](https://github.com/electronart/SearchCloudOne/wiki/Index-Technologies)

### Example Request

```json
{
  "APIKey": "ba020ccd-80bc-463c-8b1d-984544c1478a",
  "Action": "create-index",
  "Name": "Business Letters",
  "Description": "Business Letter Templates",
  "Tech": 1
}
```

### Example Response

```json
{
  "IndexUUID": "89c0608b-4b51-4a66-8cbc-64682df7b270"
}
```

## Action `DeleteIndex`

Delete an Index, including all of the files belonging to that Index. If an Index is deleted, it cannot be restored by users programmatically.

An `IndexUUID` should be passed representing the Index that should be deleted.

### Example Request
```json
{
  "Action": "DeleteIndex",
  "IndexUUID": "05fb01f1-4f69-4637-bb8b-50d6fcce0c1e"
}
```
### Example Response
Response body will be empty. Check status code is `200 OK` for success.


## Action `AppendMetadataSpecToIndex`

Specify what additional fields, optional and required, should be stored with each document in the index.

Expects the following:
* `IndexUUID` The UUID of the Index the specification applies to
* `Fields` An array of objects, each containing the following:-
  * `Name` The name of the field
  * `MetaType` The type of meta as defined here (TODO)
  * `Required` whether or not the field is required. File uploads will fail if the field is not passed with the file.
  * `DefaultValue` if no value is passed, a default value. Default Value is _ignored_ for _required_ fields which must always be passed. If blank, assumed none.
  * `ExtraParams` any extra params as defined here(TODO)

### Example Request
```json
{
  "APIKey": "bfa8e6cb-83e1-4511-a4b7-772784bb4818",
  "action": "append-metadata-spec-to-index",
  "IndexUUID": "833fbf0a-2960-44fa-a9a9-ef82665fab36",
  "Fields": [
    {
      "Name": "Date Of Judgement",
      "MetaType": "Date",
      "Required": true,
      "DefaultValue": "",
      "ExtraParams": "TODO ExtraParam for DateRange"
    },
    {
      "Name": "Date Of Ammendment",
      "MetaType": "Date",
      "Required": false,
      "DefaultValue": "",
      "ExtraParams": "TODO"
    },
    {
      "Name": "Case Title",
      "MetaType": "String",
      "Required": true,
      "DefaultValue": "",
      "ExtraParams": ""
    }
  ]
}
```

## Example Response

No JSON Data in Response. Check Response Status Code is *200* OK for success.