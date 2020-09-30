### Roles

<!-------------------- LIST ROLES -------------------->

#### List (namespace) Roles

```shell
curl -X GET \
   -H "MC-Api-Key: your_api_key" \
   "https://cloudmc_endpoint/v1/services/a_service/an_environment/roles"
```

> The above command returns a JSON structured like this:

```json
{
    "data": [
        {
            "id": "role-id",
            "age": "1w4d",
            "metadata": {
                "creationTimestamp": "2020-09-18T10:01:26.000-04:00",
                "name": "kubeadm:bootstrap-signer-clusterinfo",
                "namespace": "kube-public",
                "uid": "57ef0321-e44a-43c9-9f2a-e448c3a66a46"
            },
            "rules": [
                {
                    "apiGroups": [],
                    "resourceNames": [],
                    "resources": [],
                    "verbs": []
                }
            ]
        }
    ],
    "metadata": {
        "recordCount": 1
    }
}
```

<code>GET /services/<a href="#administration-service-connections">:service_code</a>/<a href="#administration-environments">:environment_name</a>/roles</code>

Retrieve a list of all roles in a given [environment](#administration-environments).

| Attributes                                 | &nbsp;                                                                                       |
| ------------------------------------------ | -------------------------------------------------------------------------------------------- |
| `id` <br/>_string_                         | The id of the role.|
| `metadata` <br/>_object_                   | The metadata of the role.|
| `metadata.creationTimestamp` <br/>_string_ | The date of creation of the service account as a string.|
| `metadata.name` <br/>_string_              | The name of the role.|
| `metadata.namespace` <br/>_string_         | The namespace in which the role is created.|
| `metadata.uid` <br/>_object_               | The UUID of the role.|
| `rules` <br/>_array_               | The array of rules assocaited with this role.|

Note that the list is not complete, since it is refering to the [kubernetes api details](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md).

<!-------------------- GET A ROLE -------------------->

#### Get a role

```shell
curl -X GET \
   -H "MC-Api-Key: your_api_key" \
   "https://cloudmc_endpoint/v1/services/a_service/an_environment/roles/role-name/role-namespace"
```

> The above command returns a JSON structured like this:

```json
{
    "data": {
        "id": "kubeadm:bootstrap-signer-clusterinfo",
        "age": "1w4d",
        "apiVersion": "rbac.authorization.k8s.io/v1",
        "kind": "Role",
        "metadata": {
            "creationTimestamp": "2020-09-18T10:01:26.000-04:00",
            "name": "kubeadm:bootstrap-signer-clusterinfo",
            "namespace": "kube-public",
            "uid": "57ef0321-e44a-43c9-9f2a-e448c3a66a47"
        },
        "rules": [
            {
                "apiGroups": [],
                "resourceNames": [],
                "resources": [],
                "verbs": []
            }
        ]
    }
}
```

<code>GET /services/<a href="#administration-service-connections">:service_code</a>/<a href="#administration-environments">:environment_name</a>/roles/:id</code>

Retrieve a role and all its info in a given [environment](#administration-environments).

| Attributes                 | &nbsp;                                            |
| -------------------------- | ------------------------------------------------- |
| `id` <br/>_string_         | The id of the role.                          |
| `apiVersion` <br/>_string_ | The API version used to retrieve this role.  |
| `metadata` <br/>_object_   | The metadata of the role.                    |
| `rules` <br/>_array_       | The array of rules assocaited with this role.|

Note that the list is not complete, since it is refering to the [kubernetes api details](https://github.com/kubernetes/community/blob/master/contributors/devel/sig-architecture/api-conventions.md).