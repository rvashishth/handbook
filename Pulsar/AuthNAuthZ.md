
# Auth - Pulsar

- [Auth - Pulsar](#auth---pulsar)
  - [Tenant Client(Auth Server)](#tenant-clientauth-server)
    - [Scopes](#scopes)
    - [Resources](#resources)
    - [Policy](#policy)
    - [Permission](#permission)
  - [References](#references)
  - [Questions](#questions)

## Tenant Client(Auth Server)

Each **Tenant** in pulsar is a client in auth-server, named as `tenant-client-name`

In keycloak we can convert client application to resource server. For pulsar each tenant will act as a resource server

Under each tenant we have scope, resources, policy and permissions

### Scopes

list of scope which will be associated with resources. i.e. produce consumer

### Resources

- Namespace/topic are created as resources in auth server. 
- Each resource has scope(produce/consume) associated with it

### Policy
- policy name contains the resource name, scope. `persistent://uhc/sunshine/healthdata producer permission`
- Each policy has associated list of clients(producers/consumers) who has access to this policy

### Permission

- maps a resource(topic), a scope(produce/consume) with a policy
- Basically permission says that which policy allow access to a resource + scope
- Resource + Scope + Policies

## References

https://www.keycloak.org/docs/latest/authorization_services/#_resource_server_overview

https://www.keycloak.org/docs/latest/authorization_services/#_resource_server_enable_authorization

## Questions

