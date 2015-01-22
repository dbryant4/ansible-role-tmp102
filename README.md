# route53-dyndns Role

[![Build
Status](https://travis-ci.org/dbryant4/ansible-role-route53-dyndns.svg?branch=master)](https://travis-ci.org/dbryant4/ansible-role-route53-dyndns)

## Description

This role manages the installation of [route53-dyndns](https://github.com/JacobSanford/route-53-dyndns) on a Raspberry Pi,
although it should be able to manage any other Debian based system.

## Provides

1. A node which will update an A record in AWS's Route53 every hour.

## Requires

1. Ansible 1.6 or higher
2. Raspberry Pi (possibly other Debian based systems)
3. A JSON file must exist in `/etc/ansible/facts.d` called `aws.fact`. The
   file should look similar to the following and must contain proper AWS
credentials to update an A record in AWS route53. ```{
"access_key":"AWS_ACCESS_KEY",
"secret_key":"AWS_SECRET_KEY"
}```

## Variables

- route53_dyndns_record - the A record to update within Route53
- route53_dyndns_git_url - the URL of the repo where route53-dyndns.py
  can be found. Usually this will not need to change.
- route53_dyndns_destination_path - where on the local file system
  should the route53-dyndns scripts live. Default: /root/route53-dyndns

### Changing Variable Values

To Change the value of variables, create a file in `host_vars/` or `group_vars/` or define variables in the playbook.

There are other options for changing variable values. See [Ansible
Variable
Documentation](http://docs.ansible.com/playbooks_variables.html) for
more ideas.

## Usage

Include this role in your plays and set varaibles as desired.

```yaml
---
  name: route53-dyndns-servers
  hosts: route53-dyndns-servers
  vars:
    route53_dyndns_record: 'myhome.mydomain.com'
  roles:
    - route53-dyndns
```

## Limitations

I decided not to use the built-in [cron module](http://docs.ansible.com/cron_module.html)
in an attempt to keep things simple. This role will create a script
which sets AWS credentials and runs the route53-dyndns.py script every
hour. This should be good enough for most users.

## Tests
This role includes Travis CI tests.
