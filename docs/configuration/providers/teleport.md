# Teleport
## Overview

Passage Server supports managing Teleport roles & role assignment to users

## Configuration
### Example Role
AWS provider can be directly used in providers section of your defined role. Example:
```yaml
roles:
  - name:  SRE Power User Access
    description: Allows access to monitoring systems
    approvalRuleRef:
      name: SRE approvers
    tags:
      - sre
    providers:
      - name: Teleport
        provider: teleport
        runAsync: true
        credentialRef:
          name: teleport
        parameters:
          group: sre-pu-teleport
          groupDefinition: |
            spec:
              allow:
                app_labels:
                  type:
                  - argocd
                kubernetes_groups:
                - system:masters      
                kubernetes_labels:
                  stage: dev
                kubernetes_resources:
                - kind: '*'
                  name: '*'
                  namespace: '*'
                  verbs: ['*']          
```

#### group
Teleport role name. Can contain multiple roles, comma separated e.g. `access,editor,auditor`

#### groupDefinition
Teleport role spec (optional)****. Does not work when multiple roles are defined inside `group` parameter

### Creds

To enable the Teleport provider, update the Passage Server configuration file:

Provider needs the minimal `creds` configuration:
```yaml
creds:
  teleport:
    data:
      credentialsfile: creds/teleport-identity-file
      hostname: teleport.example.com
```

#### credentialsfile
Create a user who can add/remove roles to users and create/update roles. For example built-in role `editor`.

To create an user `tctl` can be user:
```
tctl users add api-admin --roles editor
```

To obtain credentials file:
```
tctl auth sign --user=api-admin -o output-identity --tar
```

Create a K8s secret, which will be mounted to passage server pod:
```
apiVersion: v1
kind: Secret
metadata:
  name: teleport-identity-file
type: Opaque
stringData:
  teleport-identity-file: |-
    -----BEGIN RSA PRIVATE KEY-----
    ...
```

Take the glance at the lines
```
    -----END RSA PRIVATE KEY-----
    ssh-rsa-cert-v01@openssh.com 
    XXXXXNBASADS...
    ...
    ...
    -----END CERTIFICATE-----
    @cert-authority teleport.example.com .., ssh-rsa
    XXXXXNBASADS...  
    type=host  
```

Those should be in a single line like this:
```
    -----END RSA PRIVATE KEY-----
    ssh-rsa-cert-v01@openssh.com XXXXXNBASADS...
    ...
    ...
    -----END CERTIFICATE-----
    @cert-authority teleport.example.com .., ssh-rsa XXXXXNBASADS... type=host  
```

#### hostanme
Address of Teleport proxy server.