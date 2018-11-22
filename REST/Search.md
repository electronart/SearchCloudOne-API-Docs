This is the REST API Documentation for the _Search_ endpoint.

The _Search_ endpoint is where searches are performed.
It can also serve the HitViewer. 

# Basics
* Accepts content-type `application/json` only
* Accepts `POST` requests only

# Authentication

A Valid API Key with `Read Permission` to all specified indexes must be passed with every request.

# Arguments

* `Indexes` - An array of Indexes to search containing:-
  * `IndexID` - The ID of the Index
  * `IndexUUID` - The UUID of the Index
* `Parameters` - All Search Parameters
  * `Query` - A query string
  * `ResultsPerPage` (Optional, Default 10) - Any number between _1_ and _1024_
  * `MaxResults` (Optional, Default 500) - The maximum amount of results to retrieve - Any number between _1_ and _1000_
  * `Page` (Optional, Default 1) - The page to retrieve in the resultset.
  * `IncludeContext` (Optional, Default *false*) - Whether or not to include _Words of Context_ in the Search Result. Pass *true* to retrieve words of context.
  * `AutoStop` (Optional, Default *100,000,000*) - Automatically stops searching after a certain amount of documents have been retrieved. Speeds up searching in large indexes, may miss results on broader search terms.
  * `Stemming` (Optional, Default *false*) - Set to *true* to search with Stemming Enabled.
  * `Synonyms` (Optional, Default *false*) - Set to *true* for Synonym Searching.
  * `Version` (Optional, Default 1) - Changes the behaviour of the HitViewer. Should use Version 3 - Defaults to 1 to
    support a legacy application.
  * `HitViewer` (Optional) - Changes the nature of the request. [Read about HitViewer](/HitViewer)
  
  

### Example Request

```json
{
  "APIKey": "bfa8e6cb-83e1-4511-a4b7-772784bb4818",
  "Indexes": [
    {
      "IndexID": 456,
      "IndexUUID": "fdc54d54-9c83-4d67-ba31-1fe9931f879f"
    },
    {
      "IndexID": 702,
      "IndexUUID": "855568b8-1ad9-4517-acb1-c9d28f22c86a"
    }
  ],
  "Parameters": {
    "Query": "Lagos",
    "Page": "10"
  }
}
```

### Example Response

TODO - Some of the following information inaccurate & needs fixing.

```json
{
  "Results": [
    {
      "FileUUID": "663b569b-ab54-44fa-a45f-5449b5af7646",
      "Size": 34562353,
      "DocHitCount": 1,
      "DocHitsPos": [
        {
          "32": {}
        },
        {
          "64": {}
        }
      ],
      "TypeID": 1,
      "DateUploaded": "2018-06-06T16:56:30.883Z",
      "DocDisplayName": "Lagos Crown Court vs Supreme Chicken Overlord the Third",
      "Context": "...The Chicken layed an egg in <b>Lagos</b> Crown Court and somebody made a case about it ..."
    },
    {
      "FileUUID": "03b76481-8977-44b6-b381-09428bf48a13",
      "Size": 34562353,
      "DocHitCount": 1,
      "DocHitsPos": [
        {
          "32": {}
        },
        {
          "64": {}
        }
      ],
      "TypeID": 4,
      "DateUploaded": "2018-06-06T16:56:30.885Z",
      "DocDisplayName": "Some Title about Lagos",
      "Context": "...Something something, <b>Lagos</b> Something something..."
    }
  ]
}
```


