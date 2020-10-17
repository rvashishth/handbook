# https://oauth.net/2/

> OAuth is only defined for HTTP protocol

> Oauth2 is not backward compatible with OAuth 1

- Access Token
  - resource_access
  - roles
  - organization
  - scope

#### Roles - 4 defined roles

- Client i.e. a photo editing app
- Resource Owner i.e. A google drive user
- Resource Server i.e. google drive storage
- Authorization Server i.e. this one is trusted by google drive storage

#### Authorization Grant Types

> 4 pre defined grant types. or can be any extension grant type.

> Authorization flow may change based on the grant type

1. Authorization Code
2. Implicit
3. [Client Credential](https://tools.ietf.org/html/rfc6749#section-4.4)(used by gateway api clients)
4. Resource Owner Password Credentials

> Client can request for authorization grant directly to resource owner OR via authorization server(preferred)

#### Key elements

- Access Token
- Referesh Token

# OIDC

- userinfo api
- id_token
