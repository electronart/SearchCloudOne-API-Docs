# SearchCloudOne REST API

SearchCloudOne provides a REST API that allows you to manage Indexes and Perform Search Requests.

SearchCloudOne's API Communicates in `JSON` with `POST` requests. 

## Authentication

In order to use the SearchCloudOne REST API, you will need to [Create an API Key](https://www.searchcloudone.com/help/creating-an-api-key).

All requests to endpoints should pass a valid `APIKey` as part of the JSON data. 

##### Example

```json
{
  "APIKey" : "########-####-####-#################"
}
```

## Terminology

SearchCloudOne works by *Indexing Files* to make them Searchable.
 
*   **Indexes** - Collections of Files that are *indexed* to make the files quickly
    searchable.
*   **Files**   - The files that are uploaded into the indexes, which once indexed, can be searched and retrieved.

*IndexTechnologies* are used to Index the contents of files. Presently, SearchCloudOne uses the 
*dtSearch Engine* to provide index and search functionality. 

However, in the future SearchCloudOne may employ additional 
technologies to provide enhanced functionality and better performance as time goes on.
 
## Endpoints

SearchCloudOne's API is reached via the sub-domain `api.searchcloudone.com`

SearchCloudOne provides 2 endpoints of use to its customers: `/Files` and `/Search`

### Files

The *Files* endpoint is used to upload, download and delete *Files*

[Read the Docs](/Files)
### Search

The *Search* endpoint is used to perform *Search requests*

[Read the Docs](/Search)

.



