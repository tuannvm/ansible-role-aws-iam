# Ansible Role: AWS IAM

Create AWS users and send notification to their slack account.

## Requirements

AWS account with sufficient privileges is required.

Authentication with the AWS-related modules is handled by either specifying your access and secret key as ENV variables or module arguments.

For environment variables:

```
export AWS_ACCESS_KEY_ID='AK123'
export AWS_SECRET_ACCESS_KEY='abc123'
```

For storing these in a vars_file, ideally encrypted with ansible-vault:

```
ec2_access_key: "--REMOVED--"
ec2_secret_key: "--REMOVED--"
```

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
slack_enabled: true # enable slack notification
slack_token: <slack-token>

users:
  # full definition
  - name: test-user1 # user name, should be unique (required)
    access_key_state: create # create (default) | remove | active | inactive - what to do with access key (optional)
    groups: [frontend, backend] # list of user groups (optional)
    iam_type: user # user (default) | group | role - type of IAM resources (optional)
    password: # auto-generated
    state: # present (default) | absent | update - user's state (optional)
    slack_name: so0k # slack username, for notification (required)

  # name-provided only
  - name: test-user2
```

Notice that passwords will be generated & saved in `credentials/` folder at the current path using [password lookup](http://docs.ansible.com/ansible/latest/playbooks_lookups.html#the-password-lookup)

## Dependencies

None.

## Example Playbook

```yaml
- hosts: localhost
connection: local
gather_facts: False
vars_files:
  - vars/main.yml
roles:
  - ansible-role-aws-iam
```

*Inside `vars/main.yml`*:

```yaml
slack_enabled: true # enable slack notification
slack_token: <slack-token>

users:
  # full definition
  - name: test-user1 # user name, should be unique (required)
    access_key_state: create # create (default) | remove | active | inactive - what to do with access key (optional)
    groups: [frontend, backend] # list of user groups (optional)
    iam_type: user # user (default) | group | role - type of IAM resources (optional)
    password: # auto-generated
    state: # present (default) | absent | update - user's state (optional)
    slack_name: so0k # slack username, for notification (required)

  # name-provided only
  - name: test-user2
```

## License

MIT / BSD

## Author Information

Created by [Tuannvm](https://tuannvm.com/)