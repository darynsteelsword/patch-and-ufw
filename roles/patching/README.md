Patching
=========

This role is designed to run a patch on a server of either Redhat or Debian family.

Requirements
------------

None, bar that the destination server is of Redhat or Debian family, ie Centos/Ubuntu etc.



Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

---
- hosts: all
  sudo: yes

  roles:
    - { role: patching, tags: [ 'patching' ] }

