## Type table

|    Secret Type   |            Purpose            |
|:----------------:|:-----------------------------:|
| Opaque           | Generic secrets (most common) |
| dockerconfigjson | Container registry auth       |
| tls              | SSL/TLS certificates          |
| basic-auth       | Username/password auth        |
| ssh-auth         | SSH keys                      |
| bootstrap token  | Cluster bootstrapping         |

### Summary
Kubernetes supports several Secret types depending on the use case. The most common is Opaque for generic key-value secrets. 
Other types include dockerconfigjson for container registry authentication, TLS secrets for certificates used in HTTPS, 
basic-auth for username/password authentication, and SSH auth for SSH keys. These types help Kubernetes understand how the secret is intended to be used.

> Secret types are not about security levels—they are about intended usage and integration with Kubernetes features
