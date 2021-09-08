# Red Hat Identity Management on RH Open Environments, AWS

This set of Playbooks will deploy or destroy a VM in AWS to serve as an LDAP server.

## Using in Tower

1. Create a **Project** from `https://github.com/kenmoini/grain-tower.git`
2. Create a Machine-type **Credential** - this is the SSH Key that you'll use to create/connect to resources
3. Create an AWS-type **Credential** - your AWS credentials from RHOE
4. Create/Use a `localhost` **Inventory**
5. Create a **Job Template** - use that localhost Inventory, Project, and the intended Playbook, eg destroy or deploy.  Prompt for Credentials and Extra Vars.