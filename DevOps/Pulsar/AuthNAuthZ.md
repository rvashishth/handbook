
# IDX - Pulsar

- [IDX - Pulsar](#idx---pulsar)
  - [Tenant Client(Auth Server)](#tenant-clientauth-server)
    - [Scopes](#scopes)
    - [Resources](#resources)
    - [Policy](#policy)
    - [Permission](#permission)
  - [References](#references)
  - [Questions](#questions)

## Tenant Client(Auth Server)

Each **Tenant** in pulsar is a client in IDX, named as `tenant-client-name`

In keycloak we can convert client application to resource server. For pulsar each tenant will act as a resource server

Under each tenant we have scope, resources, policy and permissions

### Scopes

list of scope which will be associated with resources. i.e. produce consumer

### Resources

- Namespace/topic are created as resources in idx. 
- Each resource has scope(produce/consume) associated with it

### Policy
- policy name contains the resource name, scope. `persistent://uhc/sunshine/healthdata producer permission`
- Each policy has associated list of clients(producers/consumers) who has access to this policy

### Permission

- maps a resource(topic), a scope(produce/consume) with a policy
- Basically permission says that which policy allow access to a resource + scope
- Resource + Scope + Policies

## References

https://github.optum.com/link/pulsar-infra/wiki

https://www.keycloak.org/docs/latest/authorization_services/#_resource_server_overview

https://www.keycloak.org/docs/latest/authorization_services/#_resource_server_enable_authorization

## Questions

- [ ] How do we know tenant belongs to which pulsar cluster. i.e. test tenant created on PR as well on dev/test cluster
- [ ] I am still able to create tenant on pulsar without any security/ how are we going to secure the admin apis? is it Because pulsar manager talks to broker directly
- [ ] What credentials are we going to need for superuser role. like create tenant. Since tenant client is auth server, so against which auth server we are going to test super user role?

- [ ] Is the policy naming is important, like it contain the resource name and scope suffix with policy word
- [ ] Unlike policy name, i guess permission name is not important. 
- [ ] If we need to have multiple scope  on one resource, do we need to create multiple permission and a respective policy?
- [ ] Are we going to have one to many mapping between permission and policy?
- [ ] Unable to generate token, getting invalid secret error
- [ ] Are we going to have 3 permission and 3 policy for each resource. and then each client have access to multiple policies

curl --request POST \
  --url https://idx-stage.linkhealth.com/auth/realms/developer-platform/protocol/openid-connect/token \
  --header 'authorization: Basic cHVsc2FyLWNsaWVudDoyYmFkMjQ0Ni01YjRkLTQxZTYtOTYyZC04MjQ2N2VjYzQxZTgK' \
  --header 'content-type: application/x-www-form-urlencoded' \
  --data grant_type=urn:ietf:params:oauth:grant-type:uma-ticket \
  --data audience=tenant-uhc