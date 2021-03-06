## Service connections

Service connections are the services that you can create resources for (e.g. compute, object storage). Service connections are scoped to [Organizations](#administration-organizations). Thus, the relationship between a service connection and an organization can be either **ownership** _(where the organization owns the connection)_ or **assignation** _(where the connection is assigned to an organization but is owned by another)_ [Environments](#administration-environments) are created for a specific service which allows you to create and manage resources within that service.

<!-------------------- LIST SERVICE CONNECTIONS -------------------->
### List service connections

`GET /services/connections?organizationId=9571b279-abaf-4a37-9f39-85d1a915af7d`

```json
{
  "data":[{
    "id": "adfbdb51-493b-45b1-8802-3f6327afb9e6",
    "serviceCode": "compute-qc",
    "name": "Compute - Québec",
    "type": "CloudCA",
    "status": {  
      "lastUpdated": "2017-08-15T12:00:00.000Z",
      "reachable": true
    }
  }]
}
```

Attributes | &nbsp;
---- | -----------
`id`<br/>*UUID* | The id of the service connection.
`serviceCode`<br/>*string* | The service code of the service connection. It is used in the endpoint of the services API.
`name`<br/>*string* | The name of the service connection.
`type`<br/>*string* | The type of the service connection.
`status`<br/>*Object* | Status of the service connection. Tells you if the service is up.<br/>*includes*: `lastUpdated`, `reachable`.

Optional query parameters | &nbsp;
---------- | -----
`organizationId`<br/>*string* | The organization whose connections are to be retrieved. If this is not provided then the request defaults to the user's organization.

<!-------------------- GET SERVICE CONNECTION -------------------->

### Retrieve a service connection

`GET /services/connections/:id`

```json
{
  "data":[{
    "id": "adfbdb51-493b-45b1-8802-3f6327afb9e6",
    "serviceCode": "compute-qc",
    "name": "Compute - Québec",
    "type": "CloudCA",
    "status": {  
      "lastUpdated": "2017-08-15T12:00:00.000Z",
      "reachable": true
    }
  }]
}
```

Attributes | &nbsp;
---- | -----------
`id`<br/>*UUID* | The id of the service connection.
`serviceCode`<br/>*string* | The service code of the service connection. It is used in the endpoint of the services API.
`name`<br/>*string* | The name of the service connection.
`type`<br/>*string* | The type of the service connection.
`status`<br/>*Object* | Status of the service connection. Tells you if the service is up.<br/>*includes*: `lastUpdated`, `reachable`.

<!-------------------- GET SERVICE CONNECTION PARAMETERS -------------------->
### Retrieve connection parameters

`GET /services/descriptors/:pluginType/parameters`

```shell
# Retrieve service connection parameters

curl -X GET \
   -H "MC-Api-Key: your_api_key" \
   "https://cloudmc_endpoint/api/v1/services/descriptors/gcp/parameters"
```
> Request body example:

```json
{
  "data": [
    {
      "field": "parentType",
      "label": "gcp.service_configuration.parameters.parent_type.label",
      "options": [
        {
          "value": "folder",
          "type": "option",
          "name": "gcp.service_configuration.parameters.parent_type.folder",
          "is18n": true
        },
        {
          "value": "organization",
          "type": "option",
          "name": "gcp.service_configuration.parameters.parent_type.organization",
          "is18n": true
        }
      ],
      "required": true,
      "type": "select"
    },
    {
      "field": "parentId",
      "label": "gcp.service_configuration.parameters.parent_id.label",
      "descriptionLabel": "gcp.service_configuration.parameters.parent_id.description",
      "required": true,
      "type": "text"
    },
    {
      "field": "jsonCredentials",
      "label": "gcp.service_configuration.parameters.json_credentials.label",
      "required": true,
      "type": "textarea"
    },
    {
      "field": "billingAccountId",
      "label": "gcp.service_configuration.parameters.billing_account.label",
      "descriptionLabel": "gcp.service_configuration.parameters.billing_account.description",
      "required": true,
      "type": "text"
    }
  ]
}
```

Attributes | &nbsp;
---- | -----------
`data`<br/>*Array* | An array of connection parameters, each with the following properties.
`field`<br/>*string* | The name of connection parameter.
`type`<br/>*string* | The type of the connection parameter. Possible values are `text`, `textarea`, `select`, `checkbox`.
`options`<br/>*Array[select options]* | A list of possible options for parameters of `type` = `select`
`required`<br/>*boolean* | An indication as to whether or not this parameter is a required one when creating a service connection.

This call returns a list of parameters that must be provided _(when creating a new `ServiceConnection`)_ for CloudMc to be able to successfully connect to service. While, the response includes a variety of information, the `field` attribute of each paramter object returned is the parameter identifier. This is used together with some value to provide the connection parameters when [creating a new `ServiceConnection`](#administration-create-service-connection).

<!-------------------- CREATE SERVICE CONNECTION -------------------->

### Create service connection

`POST /services/connections`

```shell
# Create a service connection

curl -X POST "https://cloudmc_endpoint/api/v1/services/connections" \
   -H "MC-Api-Key: your_api_key" \
   -H "Content-Type: application/json" \
   -d "request_body"
```
> Request body example:

```json
{
   "name":"GCP-AmericasRegion",
	 "type":"gcp",
	 "serviceCode":"gcp-americas",
   "organization": {
		 "id": "9571b279-abaf-4a37-9f39-85d1a915af7d"
	 },
   "parameters":[
      {
         "parameter":"parentType",
         "value":"folder"
      },
      {
         "parameter":"parentId",
         "value":"9123843023"
      },
      {
         "parameter":"jsonCredentials",
         "value":"<GCP-SERVICE-ACCOUNT-CREDENTIALS>"
      },
      {
         "parameter":"billingAccountId",
         "value":"141F7D-FDK3D0-F99HW2"
      },
      {
         "parameter":"billingSubAccountsEnabled",
         "value":""
      },
      {
         "parameter":"billingDatasetName",
         "value":"cmc_usage"
      }
   ]
}
```

Create a new service connection in a specific organization. You will need the `Reseller connections` permission to execute this operation.

Required | &nbsp;
-------- | -----------
`name`<br/>*string* | The name of the new service connection.
`type`<br/>*string* | The type of new service connection. _(e.g. gcp, azure, aws, cloudca, swift, etc.)_
`serviceCode`<br/>*string* | A globally unique code that identifies this service connection.
`organization`<br/>*[Organization](#administration-organizations)* | The organization that the service connection should be created in. <br/>*required attributes of the organization*: `id`
`parameters`<br/>*Array[[connection parameters](#administration-retrieve-connection-parameters)]* | An array of connection parameters specific to this connection type along with their values. This array should contain all `required` parameters from the list returned from the [retrieve connection parameters](#administration-retrieve-connection-parameters) API. <br/>*required attributes per parameter*: `parameter`, `value`.

<!-------------------- TEST NEW SERVICE CONNECTION -------------------->

### Test new service connection

`POST /services/connections/test`

```shell
# Test a new service connection

curl -X POST "https://cloudmc_endpoint/api/v1/services/connections/test" \
   -H "MC-Api-Key: your_api_key" \
   -H "Content-Type: application/json" \
   -d "request_body"
```
> Request body example:

```json
# requires the same request body 
# as outlined in the 'Create service connection' section
```

Test a new service connection before creating it. The request body is the same as that which is used when creating a new service connection.

<!-------------------- TEST EXISTING SERVICE CONNECTION -------------------->

### Test existing service connection

`POST /services/connections/:id/test`

```shell
# Test a new service connection

curl -X POST "https://cloudmc_endpoint/api/v1/services/connections/9571b279-abaf-4a37-9f39-85d1a915af7d/test" \
   -H "MC-Api-Key: your_api_key" \
   -H "Content-Type: application/json"
```

Test an existing service connection. This request does not require a body. The `connection id` is passed as a path parameter in the request.

<!-------------------- GET CONNECTION POLICY DESCRIPTORS -------------------->

### Retrieve connection policy descriptors

`GET /services/connections/:id/policies/descriptors`

Query Parameters | &nbsp;
---------- | -----
`section`<br/>*string* | The name of the policy section to load. Only the policy section matching the section name provided will return all required FormElements and the connection entity.

```shell
# Retrieve connection policy descriptors
curl "https://cloudmc_endpoint/api/v1/services/connections/03bc22bd-adc4-46b8-988d-afddc24c0cb5/policies/descriptors?section=trials" \
   -H "MC-Api-Key: your_api_key"
```
> The above command returns a JSON structured like this:

```json
{
  "data":[{
    "name": "general",
    "label": "general",
    "optional": false,
  },{
    "name": "trials",
    "formElements": [{
      "label": "cloudstack.service_configuration.policies.trials.network_config_for_trial_env.label",
      "descriptionLabel": "cloudstack.service_configuration.policies.trials.network_config_for_trial_env.description",
      "interpolation": {},
      "options": [{
        "value": "isolatedNetwork",
        "type": "option",
        "name": "cloudstack.service_configuration.policies.trials.network_config_for_trial_env.options.isolated_network",
        "is18n": true
      }, {
        "value": "singleNetwork",
        "type": "option",
        "name": "cloudstack.service_configuration.policies.trials.network_config_for_trial_env.options.single_subnet_vpc",
        "is18n": true
      }],
      "disabled": false,
      "required": true,
      "field": "trials:network_config_for_trial_envs",
      "reloadOnChange": false,
      "sectionsToReload": [],
      "type": "select"
    }],
    "entity": {
      "id": "7486dfbf-740d-49b1-ba68-6e4b0070ebbb",
      "name": "Google Cloud Connection",
      "policies": {
        "version": "1.0",
        "cacheEnabled": "false",
        "regions": "us-east,eu-west"
      }
    }
  }]
}
```

Attributes | &nbsp;
---- | -----------
`name`<br/>*string* | The name of the policy section.
`label`<br/>*string* | The label key for the policy section name.
`formElements`<br/>*Array* | The FormElements returned by the policy section. Form elements are only returned for a given section if the query param `section` matches the name of a policy section.  
`optional`<br/>*boolean* | Specifies if the policy section is required or not.
`entity`<br/> *Object* | The service connection entity which includes a string to string map of the `ServiceConnectionPolicy::name` to `ServiceConnectionPolicy::value`.


<!-------------------- UPDATE CONNECTION POLICIES -------------------->

### Update connection policies

`PUT /services/connections/:id/policies`

```shell
curl -X PUT \
   -H "Content-Type: application/json" \
   -H "MC-Api-Key: your_api_key" \
   -d "[{"name": "serviceVersion", "value": "1.1"}, {"name": "cacheEnabled", "value": "true"}]" \
   "https://cloudmc_endpoint/api/v1/services/connections/03bc22bd-adc4-46b8-988d-afddc24c0cb5/policies"
```
> The above command returns a JSON structured like this:


```json
{
  "data":[
    {
      "id": "003c32af-e4e0-440c-871e-f20a81e6c0b7",
      "name": "serviceVersion",
      "value": "1.1"
    },
    {
      "id":"a31f100a-f7c8-43d5-b64d-ee2db466f37d",
      "name": "cacheEnabled",
      "value": "true"
    },
    {
      "id": "c681774a-a96c-4dff-a2a6-2e9651459de7",
      "name": "regions",
      "value":"us-east,eu-west"
    }]
}
```

Attributes | &nbsp;
---- | -----------
`name`<br/>*string* | The name of the policy.
`value`<br/>*string* | The policy value.

<!-------------------- LIST ASSIGNED ORGANIZATIONS CONNECTIONS -------------------->
### List organizations assigned to a service connection

`GET /services/connections/:id/assigned_organizations?organizationId=:orgId`

```json
{
  "data": [
    {
      "id": "679fe078-a67a-4383-ab4e-51f5ef3a9287",
      "name": "MyOrg",
      "entryPoint": "my-org",
      "state": "PROVISIONED",
      "quota": {
        "id": "90c3e469-71c7-4231-b27b-31dcf3a820ad",
        "name": "Unlimited"
      }
    },
    {
      "id": "9571b279-abaf-4a37-9f39-85d1a915af7d",
      "name": "Sysco Labs",
      "entryPoint": "syscolab",
      "state": "PROVISIONED",
      "quota": {
        "id": "66a773e8-a7e9-498b-a01d-68c8f9df2fd4",
        "name": "trial"
      }
    },
    {
      "id": "d84125c6-1417-4aa3-8293-a5ddde2bfeac",
      "name": "Symcentric",
      "entryPoint": "symcentric",
      "state": "PROVISIONED",
      "quota": {
        "id": "66a773e8-a7e9-498b-a01d-68c8f9df2fd4",
        "name": "trial"
      }
    },
  ]
}
```

Returns a list of organizations to which the service connection is assigned. Only organizations that the caller has access to will be returned. _Read description of the `organizationId` query parameter below for more details about the returned list._

Attributes | &nbsp;
---- | -----------
`id`<br/>*UUID* | The id of the organization.
`name`<br/>*string* | The name of the organization.
`entryPoint`<br/>*string* | The entry point of the organization. Also refered to as the organization code.
`state`<br/>*string* | The provisioning state of the organization on the backend service. States: `PENDING`, `PROVISIONING`, `PROVISIONED`, `PENDING_PURGE`, `PURGING`, `PURGED`.
`quota`<br/>*Object* | The quota assigned to the organization (may be null depending on if the connection supports quotas or not).<br/>*includes*: `id`, `name`

Optional query parameters | &nbsp;
---------- | -----
`organizationId`<br/>*string* | The organization from whose scope the request is being made. Based on this the returned list may vary. Organizations that are above this organization _(represented by the id)_ in the organization tree hierarchy will be filtered from the returned list even if they have the connection assigned to them. If this is not provided then the request defaults to the user's organization.
