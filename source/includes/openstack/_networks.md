### Networks

Networks are privisioned to be able to control data flow to instances. To comminucate with the external network, the network must be connected to a [router]#routers

#### List networks

```shell
curl -H "MC-Api-Key: your_api_key" \
    "https://api.your.cloudmc/v1/services/compute-os/devel/networks"
```
```json
{
    "data": [
        {
            "id": "a9e8861d-64a1-4126-b385-6e4db9a5814f",
            "name": "default",
            "cidr": "10.64.0.0/22",
            "dhcpEnabled":true
        }
    ],
    "metadata": {
        "recordCount": 1
    }
}
```

<code>GET /services/<a href="#service-connections">:service_code</a>/<a href="#environments">:environment_name</a>/networks</code>

Retrieve a list of all networks in an environment.

| Attribute                  | Description                          |
| -------------------------- | ------------------------------------ |
| `id`<br/>*UUID*            | The network's id              |
| `name`<br/>*string*        | The network's name            |
| `cidr`<br/>*string* | CIDR of the network |
| `dhcpEnabled`<br/>*boolean* | If DHCP is handled by the network |
| `gateway`<br/>*string* | The IP of the gateway |
| `ipVersion`<br/>*string* | V4 or V6 |
| `subnetId`<br/>*string* | The id of the OpenStack subnet it maps to|

#### Retrieve a network

```shell
curl -H "MC-Api-Key: your_api_key" \
    "https://api.your.cloudmc/v1/services/compute-os/devel/networks/a9e8861d-64a1-4126-b385-6e4db9a5814f"
```
```json
{
    "data": {
        "id": "a9e8861d-64a1-4126-b385-6e4db9a5814f",
        "name": "default",
        "cidr": "10.64.0.0/22",
        "dhcpEnabled":true
    }
}
```

<code>GET /services/<a href="#service-connections">:service_code</a>/<a href="#environments">:environment_name</a>/networks/:id</code>

Retrieve information about a network.

| Attribute                  | Description                          |
| -------------------------- | ------------------------------------ |
| `id`<br/>*UUID*            | The network's id              |
| `name`<br/>*string*        | The network's name            |
| `cidr`<br/>*string* | CIDR of the network |
| `dhcpEnabled`<br/>*boolean* | If DHCP is handled by the network |
| `gateway`<br/>*string* | The IP of the gateway |
| `ipVersion`<br/>*string* | V4 or V6 |
| `subnetId`<br/>*string* | The id of the OpenStack subnet it maps to|

#### Create a network

```shell
curl -X POST \
    -H "MC-Api-Key: your_api_key" \
    -H "Content-Type: application/json" \
    -d "request_body" \
    "https://api.your.cloudmc/v1/services/compute-os/devel/networks"
# Request should look like this:
```
```json
{
    "name": "web",
    "cidr": "10.50.0.0/22",
    "dhcpEnabled":true
}
```

<code>POST /services/<a href="#service-connections">:service_code</a>/<a href="#environments">:environment_name</a>/networks</code>

Create a network in an environment.

Optional | &nbsp;
------ | -----------
`name`<br/>*string* | Name of the network
`cidr`<br/>*string* | The CIDR of the network. The mask should be between 1 and 30 inclusively. Example: 10.50.0.0/22
`dhcpEnabled`<br/>*boolean* | If DHCP should be enabled for the network or if it will be handled by the user


#### Delete a network

```shell
curl -X DELETE \
    -H "MC-Api-Key: your_api_key" \
    "https://api.your.cloudmc/v1/services/compute-os/devel/networks/a9e8861d-64a1-4126-b385-6e4db9a5814f"
```

<code>DELETE /services/<a href="#service-connections">:service_code</a>/<a href="#environments">:environment_name</a>/networks/:id</code>

Delete a network.