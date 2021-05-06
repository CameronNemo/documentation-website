---
layout: default
title: User Impersonation
parent: Access Control
grand_parent: Security
nav_order: 20
---

# User impersonation

User impersonation allows specially privileged users to act as another user without knowledge of nor access to the impersonated user's credentials.

Impersonation can be useful for testing and troubleshooting, or for allowing system services to safely act as a user.

Impersonation can occur on either the REST interface or at the transport layer.


## REST interface

To allow one user to impersonate another, add the following to `opensearch.yml`:

```yml
opensearch_security.authcz.rest_impersonation_user:
  <AUTHENTICATED_USER>:
    - <IMPERSONATED_USER_1>
    - <IMPERSONATED_USER_2>
```

The impersonated user field supports wildcards. Setting it to `*` allows `AUTHENTICATED_USER` to impersonate any user.


## Transport interface

In a similar fashion, add the following to enable transport layer impersonation:

```yml
opensearch_security.authcz.impersonation_dn:
  "CN=spock,OU=client,O=client,L=Test,C=DE":
    - worf
```


## Impersonating Users

To impersonate another user, submit a request to the system with the HTTP header `opensearch_security_impersonate_as` set to the name of the user to be impersonated. A good test is to make a GET request to the `_opensearch/_security/authinfo` URI:

```bash
curl -XGET -u 'admin:admin' -k -H "opensearch_security_impersonate_as: user_1" https://localhost:9200/_opensearch/_security/authinfo?pretty
```