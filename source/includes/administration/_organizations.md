## Organizations
Organizations are the largest logical grouping of users, environments and resources available in CloudMC. Each organization is isolated from other organizations. It has its own subdomain (`[entryPoint].CloudMC`) and is protected by its own customizable system [roles](#administration-roles). An administrator that must manage it's sub-organizations environments or provisioned resources can do so by having the `Access other levels` permission. Additionally, provisioned resource usage is metered at the organization level facilitating cost tracking.


<!-------------------- LIST ORGANIZATIONS -------------------->
### List organizations

`GET /organizations`

Retrieves a list of organizations visible to the caller. In most cases, only the caller's organization will be returned. However if the caller's organization has sub-organizations, and the caller has the `Access other levels` permission, the sub-organizations will be returned as well.

```shell
# Retrieve visible organizations
curl "https://cloudmc_endpoint/v2/organizations" \
   -H "MC-Api-Key: your_api_key"

# Response body example
```
```json
{
   "data": [
      {
         "id": "03bc22bd-adc4-46b8-988d-afddc24c0cb5",
         "name": "Umbrella Corporation",
         "entryPoint": "umbrella",
         "billableStartDate": "2017-08-15T12:00:00.000Z",
         "isBillable": true,
         "tags": ["a-tag"],
         "parent": {
            "id": "8e3393ce-ee63-4f32-9e0f-7b0200fa655a",
            "name": "Capcom"
         },
         "environments": [
            {
               "id": "9df14056-51e2-4000-ab14-beeaa488500d"
            }
         ],
         "roles": [
            {
               "id": "cdaaa9d0-304e-4063-b1ab-de31905bdab8"
            }
         ],
         "serviceConnections":[
            {
               "id":"11607a49-9691-40fe-8022-2e148bc0d720",
               "serviceCode":"compute-qc",
               "state": "PROVISIONED",
            }
         ],
         "users": [
            {
               "id":"0c3ffcce-a98d-4159-b6fc-04edd34e89b7",
               "userName":"wbirkin"
            }
         ]
      }
   ]
}
```

Attributes | &nbsp;
---- | -----------
`id`<br/>*UUID* | ---
`name`<br/>*string* | ---
`entryPoint`<br/>*string* | The entry point of the organization is the subdomain of the organization in the CloudMC URL : `[entryPoint].CloudMC`
`billableStartDate`<br/>*string* | The billable start date in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) of the organization
`isBillable`<br/>*boolean* | If the organization is billable this values is true, false otherwise
`tags`<br/>*Array[string]* | Tags associated to the organization
`parent`<br/>*[Organization](#administration-organizations)* | If the organization is a sub-organization, it will have it's `parent` organization. *includes*:`id`,`name`
`environments`<br/>*Array[[Environment](#administration-environments)]* | The environments belonging to the organization<br/>*includes*: `id`
`roles`<br/>*Array[[Role](#administration-roles)]* | The system and environments roles belonging to the organization<br/>*includes*: `id`
`serviceConnections`<br/>*Array[[ServiceConnection](#administration-service-connections)]* | The services for which the organization is allowed to provision resources<br/>*includes*: `id`,`serviceCode`
`users`<br/>*Array[[User](#administration-users)]* | The users of the organization<br/>*includes*: `id`

<!-------------------- FIND ORGANIZATION -------------------->
### Retrieve an organization

`GET /organizations/:id`

Retrieve an organization's details

```shell
# Retrieve an organization
curl "https://cloudmc_endpoint/v2/organizations/03bc22bd-adc4-46b8-988d-afddc24c0cb5" \
   -H "MC-Api-Key: your_api_key"

# Response body example
```
```json
{
   "data": {
      "id": "03bc22bd-adc4-46b8-988d-afddc24c0cb5",
      "name": "Nintendo US",
      "entryPoint": "nintendo-us",
      "billableStartDate": "2017-08-15T12:00:00.000Z",
      "isBillable": true,
      "tags": ["a-tag"],
      "parent": {
         "id": "8e3393ce-ee63-4f32-9e0f-7b0200fa655a",
         "name": "Nintendo"
      },
      "environments": [
         {
           "id": "9df14056-51e2-4000-ab14-beeaa488500d"
         }
      ],
      "roles": [
         {
           "id": "cdaaa9d0-304e-4063-b1ab-de31905bdab8"
         }
      ],
      "serviceConnections": [
         {
            "id":"11607a49-9691-40fe-8022-2e148bc0d720",
            "serviceCode":"compute-qc",
            "state": "PROVISIONED",
         }
      ],
      "users": [
         {
            "id":"0c3ffcce-a98d-4159-b6fc-04edd34e89b7",
            "userName":"reggie"
         }
      ]
  }
}
```

Attributes | &nbsp;
---- | -----------
`id`<br/>*UUID* | ---
`name`<br/>*string* | ---
`entryPoint`<br/>*string* | The entry point of the organization is the subdomain of the organization in the CloudMC URL :<br/>`[entryPoint].CloudMC`
`billableStartDate`<br/>*string* | The billable start date in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) of the organization
`isBillable`<br/>*boolean* | If the organization is billable this values is true, false otherwise
`tags`<br/>*Array[string]* | Tags associated to the organization
`parent`<br/>*[Organization](#administration-organizations)* | If the organization is a sub-organization, it will have it's `parent` organization. *includes*:`id`,`name`
`environments`<br/>*Array[[Environment](#administration-environments)]* | The environments belonging to the organization<br/>*includes*: `id`
`roles`<br/>*Array[[Role](#administration-roles)]* | The system and environments roles belonging to the organization<br/>*includes*: `id`
`serviceConnections`<br/>*Array[[ServiceConnection](#administration-service-connections)]* | The services for which the organization is allowed to provision resources<br/>*includes*: `id`,`serviceCode`
`users`<br/>*Array[[User](#administration-users)]* | The users of the organization<br/>*includes*: `id`

<!-------------------- CREATE ORGANIZATION -------------------->
### Create organization

`POST /organizations`

Creates a new organization as a sub-organization of the caller's organization, or a sub-organization of the specified `parent` or at the top level if the calling user has an unscoped permission. The caller requires the `Organizations create` permission.

Any service connections that are supplied in the request will be assigned to the organization asynchronously. The state of the connections assigned to the organization and its progress is reflected in the organization service connections.

The task id returned in the response below can also be used to track the completion of the asynchronous assignation of connections to this organization. 

```shell
# Create an organization
curl -X POST "https://cloudmc_endpoint/v2/organizations" \
   -H "MC-Api-Key: your_api_key" \
   -H "Content-Type: application/json" \
   -d "[request_body]"

# Request body example
```
```json
{
   "entryPoint":"umbrella",
   "name":"Umbrella Corp",
   "serviceConnections":[
      {
         "id":"9acb3b76-d5d0-420c-b075-ef320b7e5a3e"
      }
   ],
   "parent" : {
      "id":"bc0ceecf-feb5-412c-ab6e-a8df8eb7fbbd"
   }
}
```

Required | &nbsp;
---- | ----
`name`<br/>*string*  | The name of the organization. (Add info about restrictions)
`entryPoint`<br/>*string* | The entry point of the organization is the subdomain of the organization in the CloudMC URL : `[entryPoint].CloudMC`


Optional | &nbsp;
---- | ----
`serviceConnections`<br/>Array[[ServiceConnection](#administration-service-connections)] | A list of service connections for which the organization may provision resources.<br/>*required :*`id`
`parent`<br/>[Organization](#administration-organization) | The organization that will be the parent of the new organization. By default, it will default to the caller's organization.<br/>*required :*`id`

The responses' `data` field contains the created [organization](#administration-organizations) with it's `id`.

Response | &nbsp;
---------- | -----------
`taskId`<br/>*UUID* | The id of the task
`taskStatus`<br/>*string* | The status of the task
`data`<br/>*[Organization](#administration-organizations)* | The information about the created organization


```shell
# Response body example
```
```json
{
  "data": {
    "lineage": "85487519-54e3-4dad-9c42-3a5ff7f1a359",
    "quotas": [
      {
        "name": "Unlimited",
        "id": "09244b06-5b89-43fe-b972-016a590eeb38",
        "serviceConnection": {
          "id": "8901494c-01ee-4d6b-bd12-cd5347127039"
        }
      }
    ],
    "hashAlgorithmName": "SHA-512",
    "hashIterations": 100,
    "isTrial": false,
    "creationDate": "2020-04-14T18:46:15.000Z",
    "version": 2,
    "tags": [],
    "isDbAuthentication": true,
    "deleted": false,
    "serviceConnections": [],
    "ldap": {
      "version": 1,
      "deleted": false,
      "id": "0bf68b3a-8248-487b-a855-1003874339e7"
    },
    "name": "Shopify",
    "id": "85487519-54e3-4dad-9c42-3a5ff7f1a359",
    "entryPoint": "shopify"
  },
  "taskId": "fd7f6c47-d525-414c-a77f-69047966c765",
  "taskStatus": "PENDING"
}
```

<!-------------------- UPDATE ORGANIZATION -------------------->
### Update organization
`PUT /organizations/:id`

Update an organization. It's parent organization cannot be changed. Any service connections that are supplied in the request will be assigned to the organization asynchronously. The state of the connections assigned to the organization and its progress is reflected in the organization service connections.

The task id returned in the response below can also be used to track the completion of the asynchronous assignation of connections to this organization. 


```shell
# Update an organization
curl -X PUT "https://cloudmc_endpoint/v1/organizations/3f913132-d479-4332-8fee-9a8c0c01328a" \
   -H "MC-Api-Key: your_api_key" \
   -H "Content-Type: application/json" \
   -d "[request_body]"

# Request body example
```
```json
{
   "entryPoint":"umbrella",
   "name":"Umbrella Corp",
   "serviceConnections":[
      {
         "id":"9acb3b76-d5d0-420c-b075-ef320b7e5a3e"
      }
   ]
}
```

Required | &nbsp;
---- | ----
`name`<br/>*string*  | The name of the organization. (Add info about restrictions)
`entryPoint`<br/>*string* | The entry point of the organization is the subdomain of the organization in the CloudMC URL : `[entryPoint].CloudMC`

Optional | &nbsp;
---- | ----
`serviceConnections`<br/>Array[[ServiceConnection](#administration-service-connections)] | A list of service connections for which the organization may provision resources. The caller must have access to all connections that are provided. **NB :** Service connection access may be added but not revoked at this time.<br/>*required :* `id`

The responses' `data` field contains the updated [organization](#administration-organizations).


Response | &nbsp;
---------- | -----------
`taskId`<br/>*UUID* | The id of the task
`taskStatus`<br/>*string* | The status of the task
`data`<br/>*[Organization](#administration-organizations)* | The information about the updated organization


```shell
# Response body example
```
```json
{
  "data": {
    "lineage": "85487519-54e3-4dad-9c42-3a5ff7f1a359",
    "quotas": [
      {
        "name": "Unlimited",
        "id": "09244b06-5b89-43fe-b972-016a590eeb38",
        "serviceConnection": {
          "id": "8901494c-01ee-4d6b-bd12-cd5347127039"
        }
      }
    ],
    "hashAlgorithmName": "SHA-512",
    "hashIterations": 100,
    "isTrial": false,
    "creationDate": "2020-04-14T18:46:15.000Z",
    "version": 2,
    "tags": [],
    "isDbAuthentication": true,
    "deleted": false,
    "serviceConnections": [],
    "ldap": {
      "version": 1,
      "deleted": false,
      "id": "0bf68b3a-8248-487b-a855-1003874339e7"
    },
    "name": "Shopify",
    "id": "85487519-54e3-4dad-9c42-3a5ff7f1a359",
    "entryPoint": "shopify"
  },
  "taskId": "aee1862d-c187-43eb-be12-754c24022dfc",
  "taskStatus": "PENDING"
}
```


<!-------------------- DELETE ORGANIZATION -------------------->
### Delete organization
`DELETE /organizations/:id`

Delete an organization. The caller may not delete his own organization. Also, an organization may not be deleted if it has sub-organizations.

```shell
# Delete an organization
curl -X DELETE "https://cloudmc_endpoint/v1/organizations/3f913132-d479-4332-8fee-9a8c0c01328a" \
   -H "MC-Api-Key: your_api_key"
```

Returns an HTTP status code 204, with an empty response body.
