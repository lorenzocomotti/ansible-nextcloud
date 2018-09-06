Ansible Role: nextcloud 
======================================

[![Build Status](https://travis-ci.org/entercloudsuite/ansible-nextcloud.svg?branch=master)](https://travis-ci.org/entercloudsuite/ansible-nextcloud)
[![Galaxy](https://img.shields.io/badge/galaxy-entercloudsuite.nextcloud-blue.svg?style=flat-square)](https://galaxy.ansible.com/entercloudsuite/nextcloud)  

Installs nextcloud on Ubuntu 16.04 (Xenial)

## Requirements

This role requires Ansible 2.4 or higher.

## Role Variables

The role defines most of its variables in `defaults/main.yml`:

## Example Playbook

Run with default vars:

```
---

- name: run the main role
  hosts: all
  become: yes
  become_method: sudo
  become_user: root
  roles:
    - role: ansible-nextcloud
      nextcloud_websrv: nginx
      nextcloud_install_redis_server: true
      nextcloud_redis_settings:
        - { name: 'memcache.locking', value: '\OC\Memcache\Redis' }
        - { name: 'redis host', value: 'localhost' }
        - { name: 'redis port', value: '6379'}
      nextcloud_background_cron: True
      nextcloud_data_dir: "/var/ncdata"
      nextcloud_webroot: "/var/www/nextcloud/"
      nextcloud_admin_name: "admin"
      nextcloud_admin_pwd: "<your-password>"
      nextcloud_mysql_root_pwd: "<your-password>"
      nextcloud_db_name: "nextcloud"
      nextcloud_db_admin: "admin"
      nextcloud_db_pwd: "<your-password>"
      nextcloud_trusted_domain:
        - "my-domain.com"
```

## Testing

Tests are performed using [Molecule](http://molecule.readthedocs.org/en/latest/).

Install Molecule or use `docker-compose run --rm molecule` to run a local Docker container, based on the [enterclousuite/molecule](https://hub.docker.com/r/fminzoni/molecule/) project, from where you can use `molecule`.

1. Run `molecule create` to start the target Docker container on your local engine.  
2. Use `molecule login` to log in to the running container.  
3. Edit the role files.  
4. Add other required roles (external) in the molecule/default/requirements.yml file.  
5. Edit the molecule/default/playbook.yml.  
6. Define infra tests under the molecule/default/tests folder using the goos verifier.  
7. When ready, use `molecule converge` to run the Ansible Playbook and `molecule verify` to execute the test suite.  
Note that the converge process starts performing a syntax check of the role.  
Destroy the Docker container with the command `molecule destroy`.   

To run all the steps with just one command, run `molecule test`. 

In order to run the role targeting a VM, use the playbook_deploy.yml file for example with the following command: `ansible-playbook ansible-nextcloud/molecule/default/playbook_deploy.yml -i VM_IP_OR_FQDN, -u ubuntu --private-key private.pem`.  

## License

MIT
