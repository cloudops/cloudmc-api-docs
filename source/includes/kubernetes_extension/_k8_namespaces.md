### Namespaces

<!-------------------- LIST NAMESPACES -------------------->

#### List namespaces

```shell
curl -X GET \
   -H "MC-Api-Key: your_api_key" \
   "https://cloudmc_endpoint/v1/services/a_service/an_environment/namespaces?cluster_id=a_cluster_id"
```

> The above command returns a JSON structured like this:

```json
{
  "data": [
    {
      "id": "default",
      "metadata": {},
      "spec": {},
      "status": {}
    }
  ],
  "metadata": {
    "recordCount": 4
  }
}
```

<code>GET /services/<a href="#administration-service-connections">:service_code</a>/<a href="#administration-environments">:environment_name</a>/namespaces?cluster_id=:cluster_id</code>

Retrieve a list of all namespaces in a given [environment](#administration-environments).

| Required                   | &nbsp;                                                 |
| -------------------------- | ------------------------------------------------------ |
| `cluster_id` <br/>_string_ | The id of the cluster in which to list the namespaces. |

| Attributes                                 | &nbsp;                                                                                    |
| ------------------------------------------ | ----------------------------------------------------------------------------------------- |
| `id` <br/>_string_                         | The id of the namespace.                                                                  |
| `apiVersion` <br/>_string_                 | APIVersion defines the versioned schema of this representation of a namespace object      |
| `kind` <br/>_string_                       | A string value representing the REST resource this object represents                      |
| `metadata` <br/>_object_                   | The metadata of the namespace                                                             |
| `spec`<br/>_object_                        | The specification describes the attributes on a namespace.                                |
| `status`<br/>_object_                      | The status information of the namespace                                                   |

<!-------------------- GET A NAMESPACE -------------------->

#### Get a namespace

```shell
curl -X GET \
   -H "MC-Api-Key: your_api_key" \
   "https://cloudmc_endpoint/v1/services/a_service/an_environment/namespaces/cert-manager?cluster_id=a_cluster_id"
```

> The above command returns a JSON structured like this:

```json
{
  "data": {
    "id": "default",
    "apiVersion": "v1",
    "kind": "Namespace",
    "metadata": {},
    "spec": {},
    "status": {}
  }
}
```

<code>GET /services/<a href="#administration-service-connections">:service_code</a>/<a href="#administration-environments">:environment_name</a>/namespaces/:id?cluster_id=:cluster_id</code>

Retrieve a namespace and all its info in a given [environment](#administration-environments).

| Required                   | &nbsp;                                               |
| -------------------------- | ---------------------------------------------------- |
| `cluster_id` <br/>_string_ | The id of the cluster in which to get the namespace. |

| Attributes                                 | &nbsp;                                                                                    |
| ------------------------------------------ | ----------------------------------------------------------------------------------------- |
| `id` <br/>_string_                         | The id of the namespace.                                                                  |
| `apiVersion` <br/>_string_                 | APIVersion defines the versioned schema of this representation of a namespace object      |
| `kind` <br/>_string_                       | A string value representing the REST resource this object represents                      |
| `metadata` <br/>_object_                   | The metadata of the namespace                                                             |
| `spec`<br/>_object_                        | The specification describes the attributes on a namespace.                                |
| `status`<br/>_object_                      | The status information of the namespace                                                   |