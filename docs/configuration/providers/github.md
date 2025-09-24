# Github
## Overview

Passage Server supports managing Github Organisation members & role assignments

## Configuration

### Example role - adding user to org

```yaml
roles:
  - name:  SRE Power User Access
    description: Allows access to monitoring systems
    approvalRuleRef:
      name: SRE approvers
    tags:
      - sre
    providers:
      - name: Github
        provider: github
        runAsync: true
        credentialRef:
          name: github
        parameters:
          org: example
          role: member
          removeUser: false
          orgRoles:
          - all_repo_read 
          teams: |
            cto2bprimary: member
          repositories: |
            office-supplies-tracker: member        
```

### Example role - external collaborator

```yaml
roles:
  - name:  SRE Power User Access
    description: Allows access to monitoring systems
    approvalRuleRef:
      name: SRE approvers
    tags:
      - sre
    providers:
      - name: Github
        provider: github
        runAsync: true
        credentialRef:
          name: github
        parameters:
          org: example
          repositories: |
            office-supplies-tracker: member        
```

#### org
Required. GitHub organization name.

#### role
GitHub organization membership role (member or admin).
Required in case `orgRoles`, `teams` or `role` are set.

#### removeUser
If "true", the user will be removed from the org when access is revoked.

#### orgRoles
List of organization roles to assign to the user. Example: all_repo_read.

#### teams
Key-value map of GitHub teams and roles. Example: team-name: member or team-name: maintainer.

#### repositories
Key-value map of repositories and roles. Example: repo-name: admin.
If `role` is not set, user will not be added to the Github org, but be granted direct access to repository as external collaborator

### Creds

To enable the GitHub provider, you must configure credentials in the Passage Server configuration file.

Provider needs the minimal `creds` configuration:
```yaml
creds:
  github:
    data:
      appid: xxxx
      privatekeypath: creds/github-org-example-private-key    
```

#### appid
GitHub App ID

#### privatekeypath
Path to the private key file for the GitHub App