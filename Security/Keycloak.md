# Keycloak

## TODO
Policy & permission on auth server vs resource server

## Authorization Server

Keycloak provide following (ACM) - access control mechanisms

- RBAC - Role based access control
- ABAC - Attribute based access control
- UBAC - User based access control
- TBAC - Time based access control
- CBAC - Context based access control
- Rule Based Access Control(Javascript based)
- Custom access control mechanisms using Policy Provider SPI(Service Provider Interface)

Authorization process
- Resource Management
- Policy and Permission management
- Policy Enforcement

You can enable any registered client application as a resource server and start managing the resources and scopes you want to protect.

Scopes usually represent the actions that can be performed on a resource, but they are not limited to that. You can also use scopes to represent one or more attributes within a resource.

> Create a policy > define permission > apply policy to permission

Policies define the conditions that must be satisfied to access resource or scope. but they are not tied to what they are protecting. Policies are related to ACM

Once you have your policies defined, you can start defining your permissions. 

Permissions are coupled with the resource they are protecting. Here you specify what you want to protect (resource or scope) and the policies that must be satisfied to grant or deny permission.

To enable authorization service on a client application, select `openid-connect` as client protocol


Create protected **resources** and scope
Create **permission** for these resources and scope
Associate these permission with authorization **policies**

> In Keycloak, any confidential client application can act as a resource server

Resource

## Keywords
- Resource, Scope, Permission, Policy
- Scope - an action that can be performed on a resource
- Authorization Policies
- ACM - access control mechanisms 
- SPI - Service provider interface
- Resource Servers
- UMA - User managed Access
- Policy Enforcers
- RPT - Requesting Party Token


Security information for web servers is stored in session
