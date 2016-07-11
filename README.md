authorized_keys
===============

[![Ansible Galaxy][galaxy_image]][galaxy_link]
[![Build Status][travis_image]][travis_link]
[![Latest Tag][tag_image]][tag_link]

An [Ansible](https://www.ansible.com/) to manage SSH authorized_keys files.

Requirements
------------

None.

Role Variables
--------------

```yaml
# list of authorized keys specifications
authorized_keys__list: []
## authorized key
#  - key: string
## user of authorized key
#    user: string
## OPTIONAL: should be authorized key exclusive
#    exclusive: boolean
## OPTIONAL: ssh key options
#    key_options: string
## OPTIONAL: should directory containing authorized_keys file be managed by ansible
#    manage_dir: boolean
## OPTIONAL: path for authorized_keys file
#    path: string
## OPTIONAL: if set to true, authorized key is removed
#    disabled: boolean

# default value for exclusive option of authrozied keys
authorized_keys__exclusive: false

# default value for manage_dir option of authorized keys
authorized_keys__manage_dir: true
```

Dependencies
------------

None.

Example Playbook
----------------

```yaml
- hosts: all
  roles:
    - role: "tomashavlas.authorized_keys"
      authorized_keys__list:
        - key: "{{ lookup('file', 'files/public_keys/root/id_rsa.pub' }}"
          user: "root"
          exclusive: true
        - key: |
            {{ lookup('file', 'files/public_keys/example/id_rsa.pub' }}
            {{ lookup('file', 'files/public_keys/example/id_ed25519.pub' }}
          user: "example"
```

For more examples see [test cases](https://github.com/tomashavlas/ansible-role-authorized_keys/tree/master/tests).

License
-------

BSD

Author Information
------------------

Created by [Tomáš Havlas](https://github.com/tomashavlas) in 2016.

[galaxy_image]: https://img.shields.io/badge/galaxy-tomashavlas.authorized__keys-blue.svg?style=flat
[galaxy_link]: https://galaxy.ansible.com/tomashavlas/authorized_keys/
[tag_image]: https://img.shields.io/github/tag/tomashavlas/ansible-role-authorized_keys.svg
[tag_link]: https://github.com/tomashavlas/ansible-role-authorized_keys/tags
[travis_image]: https://travis-ci.org/tomashavlas/ansible-role-authorized_keys.svg?branch=master
[travis_link]: https://travis-ci.org/tomashavlas/ansible-role-authorized_keys/
