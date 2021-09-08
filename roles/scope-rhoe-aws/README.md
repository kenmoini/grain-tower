Scope Red Hat Open Environments - AWS
=========

This role provides the facts for RHOE on AWS to build upon the pre-provide zone and other resources.

Requirements
------------

- Boto3
- AWS Account

Role Variables
--------------

`aws_region` will set the target AWS region

Dependencies
------------

**Collections:**

- amazon.aws
- community.aws

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
