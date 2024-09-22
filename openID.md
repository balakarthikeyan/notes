## What is OpenID?
OpenID is an open standard and decentralized authentication protocol controlled by the OpenID Foundation.
OAuth is an open standard for access delegation.
OpenID Connect (OIDC) Combines the features of OpenID and OAuth i.e. does both Authentication and Authorization.

OpenID take the form of a unique URI managed by some "OpenID provider" i.e identity provider (idP).
OAuth can be used in conjunction with XACML where OAuth is used for ownership consent and access delegation whereas XACML is used to define the authorization policies.
OIDC uses simple JSON Web Tokens (JWT), which you can obtain using flows conforming to the OAuth 2.0 specifications. 
OAuth is directly related to OIDC since OIDC is an authentication layer built on top of OAuth 2.0.

### OpenID Connect is built on top of OAuth2.

An access_token is useful to call certain APIs in Auth0 (e.g. /userinfo) or an API you define in Auth0.
An id_token is a JWT and represents the logged in user. It is often used by your app.
A refresh_token (only to be used by a mobile/desktop app) doesn't expire (but is revokable) and it allows you to obtain freshly minted access_tokens and id_token.