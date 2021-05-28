# Getting started

The Cox Edge API allows you to manage your environments and provision resources in a simple programmatic way using standard HTTP requests.

The API is  [RESTful](https://en.wikipedia.org/wiki/Representational_state_transfer). Responses, successful or not, are returned in [JSON](http://www.json.org/). Request bodies must be [JSON](http://www.json.org/), and should be made over SSL.

API endpoint : `https://portal.coxedge.com/api/v1`

## Authentication

```shell
## To authenticate, add a header
## Make sure to replace `your_api_key` with your API key.
curl "https://portal.coxedge.com/api/v1/organizations" \
   -H "MC-Api-Key: your_api_key"
```

API endpoints are secured by the same role-based access control (RBAC) as the Cox Edge portal. To identify who is making the requests, it is required to add a header to your HTTP requests:

`MC-Api-Key: your_api_key`

<aside class="notice">
You must replace <code>your_api_key</code> with your personal API key.
</aside>

The API key is found from the API keys section under the user profile menu. If you don't see Cox Edge API keys section, contact your system administrator as you may not have the permission to see that section. **Your API key carries the same privileges as your Cox Edge account, so be sure to keep it secret**. If you think your API has been compromised, regenerate your API key from the API keys section.

## HTTP verbs
The Cox Edge API can be used by any tool that is fluent in HTTP. The appropriate HTTP method should be used depending on the desired action.

Verbs | Purpose
------ | -------
`GET` | Used to retrieve information about a resource.
`POST` | Used to create (or provision) a new resource or perform an operation on it.
`PUT` | Used to update a resource.
`DELETE` | Used to delete a resource.

## Responses

### Success response

<!--
```json
{
  "data": [
    { "_comment" : "JSON representation of first object goes here" },
    { "_comment" : "JSON representation of second object goes here" }
  ],
  "metadata": {
    "pageSize": 2,
    "pageCurrent": 1,
    "recordCount": 2,
    "sortField": "templateName",
    "sortOrder": "ASC"
  }
}
```
-->

```shell
# Example without tasks
```

```json
{
  "data": [
    { "_comment" : "JSON representation of first object goes here" },
    { "_comment" : "JSON representation of second object goes here" }
  ]
}
```

```shell
# Example of compute API call with task
```

```json
{
  "taskId": "c2c13744-8610-4012-800a-0907bea110a5",
  "taskStatus": "PENDING"
}
```

When an API request is successful, the response body will contain the `data` field with the result of the API call. If you're using asychronous APIs, the `data` field might be empty since most of the operations are asynchronous. The response will contain the `taskId` and `taskStatus` fields so that you can retrieve the result of the operation you executed through the [task API](#tasks).

Attributes | &nbsp;
--- | ---
`data` | The data field contains the object requested by the API caller.
`taskId` | The [task id](#tasks) of an operation executed through the services API.
`taskStatus` | The status of a [task](#tasks) of an operation executed the services API.
<!--
`metadata` | The metadata is an optionally returned field containing paging and sorting information
-->

<aside class="notice">
If the response contains the "errors" field, the request was <strong>not</strong> successful.
</aside>

### Error response

```shell
# Example of error response
```

```json
{
  "errors": [
    {
      "code": 2012,
      "message": "Cannot stop an instance that isn't in the running state",
      "context": {
        "id": "4534cc36-bc46-48bc-ac5c-3ee4e42f0a44",
        "currentState": "Stopped",
        "expectedStates": [
          "Running"
        ],
        "type": "instances"
      }
    }
  ]
}
```

When an API request is unsuccessful, the response body will contain the `errors` field :

Attributes | &nbsp;
--- | ---
`errors` | A list of errors objects that contain information about each error.

Each error has additional fields to describe it :

Attributes | &nbsp;
--- | ---
`code` | The Cox Edge error code.
`message` | A human readable explanation of the error code.
`context` | Additional information.

The HTTP status codes of error responses :

Status code | Reason
----------- | -------
`200` | The request was successful.
`204` | The request was successful and the response body is empty.
`400` | Bad request -- Occurs when invalid parameters are provided or when quota limit is exceeded.
`403` | Forbidden -- Not authorized to perform this request.
`404` | Not Found -- Cannot locate the specified endpoint.
`405` | Method not allowed -- Cannot use that HTTP verb on the specified endpoint.
`500` | An unexpected error occurred.
<!--
## Paging & sorting
All `GET` endpoints returning a list of objects support pagination. The desired page of result is specified by providing the following HTTP query parameters:

Name | Description
------------------- | -----------
`page_number` | The page of data to retrieve
`page_size` | The number of items to display per page
`sort_by` | The field name to sort by
`sort_order` | The sort order (ASC or DESC)
-->
