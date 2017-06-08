Ansible Role: Kibana - Basic License
===================
> Kibana >5 with an Elasticsearch basic license

# Background
* Heavily inspired by [geerlingguy/ansible-role-kibana](https://github.com/geerlingguy/ansible-role-kibana)
* It is recommended to use this in conjunction with the official [ansible-elastic](https://github.com/elastic/ansible-elasticsearch)
role. If you're using only the basic license then you should enable x_pack on the instances
and also enable monitoring on the instances
```
es_enable_xpack: true
es_xpack_features: 'monitoring'
```

* X-Pack is great but defaults to running all features: security, monitoring, altering
* However, with the basic license, only monitoring is free
* The settings need to be specific to get Kibana to work with a basic license
and for monitoring to appear
* You'll need to register for a basic license so you can apply that to the node
running Kibana. [Link to register for a basic license](https://register.elastic.co/)
You can apply the license via a curl request to a node by
```
curl -XPUT {your ip}/_xpack/license?acknowledge=true -H "Content-Type: application/json" -d @{your file}
```
* My personal setup involves running `n` number of nodes with an additional 
[coordinating](https://www.elastic.co/guide/en/kibana/current/production.html#load-balancing)
node that is also running kibana

# What This Role Does
* Installs Kibana
* Installs x-pack into Kibana
* Stars Kibana as a service
* Copies config into Kibana and restarts Kibana

# Optional Vars
* `kibana_version` defaults to `5.4.0`
* `kibana_home` defaults to `/usr/share/kibana`
* `es_major_version` defaults to `5.x`

# References/Debugging
* [X-pack and basic license thread](https://discuss.elastic.co/t/x-pack-and-basic-license/65014/7)
* This runs Kibana as a service and so Kibana debugging isn't outputted. To help
debug it is useful to view the service logs by running on the server instance
```
sudo journalctl -f -u kibana
```
* View your license by running:
```
curl {ip and host}/_xpack/license
```
This should respond with something like this:
```
{
  "license" : {
    "status" : "active",
    "uid" : "some-has",
    "type" : "basic",
    "issue_date" : "some-date",
    "issue_date_in_millis" : some-date
    "expiry_date" : "some-date",
    "expiry_date_in_millis" : some-date
    "max_nodes" : 100,
    "issued_to" : "your-name",
    "issuer" : "some-format",
    "start_date_in_millis" : some-date
  }
}
```
