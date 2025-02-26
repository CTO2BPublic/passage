# AWS
## Overview

Passage Server supports AWS Identity Center (formerly AWS SSO) for access management. This guide explains how to configure and use the AWS provider.

## Configuration

### Example Role
AWS provider can be directly used in providers section of your defined role. Example:
```yaml
roles:
  - name: SRE Power User Access
    description: Privilleged access. Provides PU access to AWS
    approvalRuleRef:
      name: SRE approvers
    tags:
      - sre
    providers:
      - name: AwsPu
        provider: aws
        runAsync: true
        credentialRef:
          name: aws
        parameters:
          group: pu-group
```

### Creds

To enable the AWS provider, update the Passage Server configuration file:

Provider needs the minimal `creds` configuration:
```yaml
creds:
  aws:
    data:
      accesskeyid: xxxxxxxxxxxx
      secretaccesskey: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
      identitystoreid: d-xxxxxxx
      instancearn: arn:aws:sso:::instance/ssoins-xxxxxxxx
      region: eu-central-1
```

#### accesskeyid
access key id of a dedicated user

#### secretaccesskey
secret access key of a dedicated user

#### identitystoreid
The **Identity Store ID** is unique to your AWS organization and is tied to AWS Identity Center.

1. **Log in to the AWS Management Console.**
2. Navigate to **AWS Identity Center** (search for "Identity Center" in the Services section).
3. In the left-hand menu, go to **Settings**.
4. Scroll down to the **Identity Source** section


#### instancearn
The **Instance ARN** refers to the ARN of the AWS Identity Center instance.

1. Open your terminal and execute the following AWS CLI command:
   ```bash
   aws sso-admin list-instances

#### region
Region of your Identity center instance