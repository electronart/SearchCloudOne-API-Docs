The Sub Account Mgmt endpoint (`/SubAccountMgmt`) is the endpoint where the master account holder can view and manage sub-users of their account.

The Endpoint accepts requests of contentType `application/json` only.

All requests must pass a valid `APIKey`. A Valid API Key is one that is either a 'Master Account' API Key, or an 'API User' Key where the given key has 'Manage Users' Permission. `Organization Member` Keys are **not** valid.

# Actions

All requests must pass a valid `Action`. The following Action's can be carried out.

* `ListAccounts` Retrieve a list of accounts belonging to the master account holder.
* `GetSubPerms`  Retrieve the permissions assigned to a sub account.
* `SetSubPerms`  Assign permissions to a sub account.
* `CreateAccount` Create an API User or Organization Member account.
* `IsFree` Check if a Username is free. (Organization Member accounts must have unique usernames)
* `CreateAPIKey` Create an API Key for User. Only works for Users of type `APIUser` **or** the Master Account.
* `RevokeAPIKey` Revoke an API Key that belongs to your account.

## `ListAccounts`

Requires no extra JSON parameters.
Responds with a list of sub accounts tied to the master account, with their `UserID`, `UserUUID`, `UserName`, `DisplayName`, `UserType` and also returns `HasPerms` which indicates whether the user has been assigned any index permissions.

See: UserTypes (TODO)

_In the case that UserType is APIUser, DisplayName and UserName are the same._

### Example Request

```json
{
  "APIKey": "7e41ef06-6c0e-477e-b7e4-3103fc41fef2",
  "Action": "ListAccounts"
}
```

### Example Response

```json
{
  "SubAccounts": [
    {
      "UserID": "251",
      "UserUUID": "b611e40a-914e-45ce-b83e-6e752722ab71",
      "UserName": "john_smith27",
      "DisplayName": "John Smith",
      "UserType": "OrgMember",
      "HasPerms": true
    },
    {
      "UserID": "346",
      "UserUUID": "0df0c143-6c24-4c14-8b1b-7d6dcbffe8c7",
      "UserName": "Android App",
      "DisplayName": "Android App",
      "UserType": "APIUser",
      "HasPerms": true
    }
  ]
}
```

## `GetSubPerms`

Requires `UserID` and `UserUUID` to be passed.
Brings back whether or not the user is a master account, if it can along with an array of `IndexPermissions`.

### Example Request

```json
{
  "APIKey": "7e41ef06-6c0e-477e-b7e4-3103fc41fef2",
  "Action": "GetSubPerms",
  "UserID": "56462",
  "UserUUID": "19adb403-5c87-4ab9-8df1-12965069acd9"
}
```

### Example Response

```json
{
  "IsMaster": false,
  "IndexPermissions": [
    {
      "IndexID": "3512",
      "IndexUUID": "d2d9cbb5-63f1-4b87-b377-424d95fef0b9",
      "IndexName": "Court Judgements",
      "Read": true,
      "Write": true,
      "Delete": false,
      "AlterProperties": false
    },
    {
      "IndexID": "4657",
      "IndexUUID": "e53910af-92da-4986-bb5b-f178c08066aa",
      "IndexName": "Laws of Nigeria",
      "Read": true,
      "Write": false,
      "Delete": false,
      "AlterProperties": false
    }
  ]
}
```
## `SetSubPerms`

_Replaces_ existing permissions with a new set of permissions on the given user. (Only works on Sub-accounts, can only be called by master account).

* `UserID` and `UserUUID` must be passed.
*  An Array of `Indexes` must be passed (Can be empty to remove all permissions). Each Item should contain:
  * `IndexID` the ID of the Index.
  * `IndexUUID` the UUID of the Index.
  * `Read` Whether or not the user can Read from the Index. (EG, Search Documents, View documents)
  * `Write` Whether or not the user can add new documents to the Index.
  * `Delete` Whether the user can delete existing documents from the index (And by extension, modify existing documents).

### Example Request

``` json
{
  "APIKey": "e9a701a4-f897-4f59-8770-334202befc3e",
  "Action": "SetSubPerms",
  "UserID": 27,
  "UserUUID": "6e524731-56b6-4c99-8a00-1ca7505968d0",
  "Indexes": [
    {
      "IndexID": 258,
      "IndexUUID": "135ba038-2643-4794-b64f-836812d355e2",
      "Read": true,
      "Write": true,
      "Delete": false
    },
    {
      "IndexID": 709,
      "IndexUUID": "4d2ce6a8-24a1-48ae-9cc3-f33a0b524bce",
      "Read": true,
      "Write": false,
      "Delete": false
    }
  ]
}
```
### Example Response

No Response Body will be sent. Check the Response Status Code for success:

* `200 OK` The permissions were updated successfully.
* `412 Precondition Failed` Required fields are missing from the request.
* `401 Unauthorized` 
  * Passed an Invalid API Key (Expired or does not have permission to change this users permissions)
  * Passed an Index that does not belong to the master account holder.
  * Passed a user that does not belong to the master account holder.

## `CreateAccount`

Requires an `AccountType` and `Username` to be passed.

AccountType can be one of two types:

* `OrgMember` - For members of an organization who would login through the SearchCloudOne Management Console.
  * If passed, the request must also pass an `Email` and `DisplayName`. 
  * `Username` must be a username that is unique to SearchCloudOne.
  * SearchCloudOne will automatically send an invitation to the passed Email Address, inviting the new user to activate their account.
  * When the user activates their account, they will set their own password.
  * Organization Members do not have access to the `Users` or `API` page.
  * Organization Members are able to reset their password and change their email address at any time. Their chosen email address is (at time of writing) **not disclosed** even to the master account holder.

* `APIUser` - An account used by programs (For instance, a Search Page on a Website, or an application).
  * If passed, `Username` can be any name unique to your account with any format.
  * It is recommended to pass a descriptive name such as 'Android Application', 'www.example.com Search Page' to help with analytics tracking and billing.

### Example Create APIUser Request

```json
{
  "APIKey": "0cf08d1d-7da5-4fa8-8fa1-c5bbe12e0bbf",
  "Action": "CreateAccount",
  "Username": "Android App",
  "AccountType": "APIUser"
}
```

### Example Create OrgMember Request

```json
{
  "APIKey": "0cf08d1d-7da5-4fa8-8fa1-c5bbe12e0bbf",
  "Action": "CreateAccount",
  "Username": "john_smith27",
  "DisplayName": "John Smith",
  "AccountType": "OrgMember",
  "Email": "john.smith@example.com"
}
```

### Responses

No Response Body will be sent. Check the Response Status Code for success:

* `200 OK` The account was created successfully.
* `409 Conflict` The passed Username is already taken. Try another.
* `412 Precondition Failed` Required fields are missing from the request
* `401 Unauthorized` Passed an Invalid API Key (Expired or does not have permission to create a sub account)
* `400 Bad Request` The passed AccountType was not recognized. (Case Sensitive)

## `IsFree`

Pass a `Username`, returns Boolean `Free` in JSON. `true` if the account name is available to be taken.

### Example Request
```json
{
  "APIKey": "e029e802-268d-4d16-b0c0-7e98ec981ca3",
  "Action": "IsFree",
  "Username": "john.smith27"
}
```

### Example Response
`200 OK` returned regardless of whether account is free. Check response json.
```json
{
  "Free": true
}
```

## `CreateAPIKey`

Pass the `UserID` and `UserUUID` of a User of type `APIUser` **or** the Master Account holder.

### Example Request

```json
{
  "APIKey": "78ee609f-ef78-4d92-bf9e-bf687e3cde76",
  "Action": "CreateAPIKey",
  "UserID": 8561,
  "UserUUID": "b49bff92-2ce9-4381-9fae-973cfd2ff7f2"
}
```

### Example Response
`200 OK`
```json
{
  "CreatedAPIKey": "03e258b3-2def-48a1-a6ae-dd855a91b52c"
}
```

**Notes** 

 * Created API Key will have the permissions of the user. 
 * Changing the permissions of the user after the key is created will effectively change the permissions of the key.         
 * Deleting the user will automatically revoke the API Key and any other API Keys created for the given user.
 * An API Key created with the Master Account details will show up as an `Administrator` Key. And will in addition be able to create/rename/delete indexes,users and API Keys.
 * An API Key is a security token and you **must** keep keys private and only give the minimum required permissions in order to keep your account secure.

## `RevokeAPIKey`

Pass the API Key you wish to revoke as `KeyToRevoke`.
The API Key will be deleted from SearchCloudOne.

**Important** Any applications that used the API-Key to access SearchCloudOne services will no longer be able to access SearchCloudOne with immediate effect.

### Example Request
```json
{
  "APIKey": "4e920c5f-900f-4cfe-909f-424721c69bb5",
  "Action": "RevokeAPIKey",
  "KeyToRevoke": "1250ffc5-30d0-4322-99e1-7803362a59ce"
}
```
### Example Response

No Response Body will be sent. Check the Response Status Code for success:

* `200 OK` The account was created successfully.
* `412 Precondition Failed` Required fields are missing from the request
* `401 Unauthorized` Passed an API Key that is invalid or does not belong to your account.

