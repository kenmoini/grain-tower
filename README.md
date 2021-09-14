# Grain Tower - Seed for your Ansible Tower

This repository provides quickstart automation in Ansible Tower with little setup.

The intended environment is Red Hat Open Environments, AWS in most cases.  For IBM Cloud there's [Blue Forge](https://github.com/kenmoini/blue-forge).

With little more than a set of AWS Keys and Machine/SSH Credentials you can deploy a number of workloads such as:

- A Bastion Host!
- GitLab
- Red Hat Identity Management
- Red Hat OpenShift Container Platform, via the Bastion Host
- [WIP] Keystone

---

## General Prerequisites

## AWS Keys

Since we're deploying resources in AWS, you'll need an AWS Access Key ID and Secret Pair - if you've requested the RHOE AWS catalog item, you'll receive this Access Key and Secret via email.

Your AWS Keys are usually stored in `~/.aws/credentials` that looks like this:

```
; https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html
[default]
aws_access_key_id     = AKISOMESTRING
aws_secret_access_key = someLongerString
```

### SSH Key Pair

You'll need an SSH Key Pair - if you don't already have one, create a set via the following command: `ssh-keygen -t rsa -b 4096`

The Private Key is secret and will be used to actually connect to created VMs, the Public Key will be passed along to the Playbooks and to AWS to create AWS EC2 Keys from.

### OpenShift Pull Secret

If deploying an OpenShift cluster, you'll need an OpenShift Pull Secret: https://console.redhat.com/openshift/install/pull-secret

---

## Getting Started

The primary entry point of this automation collection is via `./bootstrap.yaml` and a set of variable definitions to control what workloads are being deployed in tandem.  You can deploy the workloads individually and directly via their `deploy.yaml` files as well.

## Using via Ansible CLI

With your AWS keys in place and an SSH Key Pair at hand, you can create a file called `secret-vars.yaml` where you can include variable overrides - see `example.secret-vars.yaml` for standard variables used.

```bash
# secret-vars.yaml is in .gitignore so copy the example over to there to modify
cp example.secret-vars.yaml secret-vars.yaml
# modify the file
vi secrets-vars.yaml
# or
nano secrets-vars.yaml
```

You could go a step futher and even encrypt this `secret-vars.yaml` file with Ansible Vault.

Next you'll run the Bootstrap Playbook with that variable file:

```bash
ansible-playbook -e "@secret-vars.yaml" bootstrap.yaml
```

---

## Using in Ansible Tower

### 1. Create a Project

All things start with a Project in Ansible Tower - create a new Project with this repo as a source: https://github.com/kenmoini/grain-tower

### 2. Create your Machine Credentials

Take your Private Key and create a new Machine type Credential in Ansible Tower

### 3. Create your AWS Credentials

Next, create your AWS type Credentials in Ansible Tower.

### 4. Create a localhost Inventory

You'll need an Inventory for the local Ansible Tower host without using SSH credentials - in the Inventory Extra Variables, provide the following:

```yaml
---
connection: local
ansible_connection: local
ansible_python_interpreter: /usr/bin/python
```

Make a Host with the Hostname `localhost` and those same Extra Variables.

### 5. Create the Job Template

With the Credentials, Inventory, and Project setup and synced, you can now create a Job Template to deploy the various workloads.

- Give it a Name, something like `Grain Tower - Deploy Workloads to RHOE AWS`
- Set the Inventory to the localhost Inventory
- Select the Project
- Choose the `bootstrap.yaml` Playbook (unless making individual Job Templates for each workload, extra work and not able to compose as Workflow Jobs due to Credential substitution limitations)
- In the Extra Variables section, you can provide the same variables as demonstrated in `example.secret-vars.yaml` to override the executed Playbook defaults.  Any other variables can also be overriden here
- Optionally, instead of simply defining all Extra Variables to compose the deployment, set them as Survey inputs to be set by the executing user