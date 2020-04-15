# Ansible Role : tranquilit.waptrepo

## Description

That role aims to configure WAPT Linux agent repository replication feature.

When applied, the configuration enables repository replication for WAPT packages, WAPT hosts packages and Windows Updates.

## Requirements

* Debian Linux or CentOS Virtual Machine
* Ansible 2.8+
* tranquilit.waptagent

## Configure WAPT repo on WAPT Linux agents

* git clone and install that role in `/etc/ansible/roles`

```
cd /etc/ansible/roles/
git clone https://www.github.com/tranquilit/ansible.waptrepo
```

* add your hosts to Ansible hosts file
```
[srvwapt]
computer1.mydomain.lan ansible_host=192.168.1.50
computer1.mydomain.lan ansible_host=192.168.1.60
```
* create a playbook with the following content in `/etc/ansible/playbooks/wapt.yml` :

```
- hosts: srvwapt
  roles:
    - { role: tranquilit.waptrepo }
```
* run your playbook with the following command :
```
cd /etc/ansible/
ansible-playbook -i hosts playbooks/wapt.yml
```

That's it WAPT agent are configured to synchronize WAPT packages!

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

### key and cert

You must replace `files/cert.pem` and `files/key.pem` by your SSL/TLS certs for Nginx.


### nginx vars

Nginx configuration parameters default are :

    centos_nginx_conf: "/etc/nginx/conf.d/wapt.conf"
    debian_nginx_conf: "/etc/nginx/sites-enabled/wapt.conf"
    nginx_ssl_path: "/etc/nginx/ssl/"
    nginx_cert: "{{ nginx_ssl_path }}cert.pem"
    nginx_key: "{{ nginx_ssl_path }}key.pem"
    wapt_get_ini: "/opt/wapt/wapt-get.ini"

### wapt-get.ini vars

The `[repo-sync]` section of `wapt-get.ini` is configured with the following variables, by default :

    enable_remote_repo: "True"
    local_repo_path : "/var/www/html"
    local_repo_time_for_sync_start : "20:30"
    local_repo_time_for_sync_end : "05:30"
    local_repo_sync_task_period : "25"
    local_repo_limit_bandwidth : "4"
    remote_repo_dirs : "wapt,waptwua"

Modify the values to fit your needs.

## Dependencies

- tranquilit.waptagent

## Example Playbook

    - hosts: hosts
      vars_files:
        - vars/main.yml
      roles:
        - tranquilit.waptrepo

Inside `vars/main.yml`:

    local_repo_time_for_sync_start : "20:00"
    local_repo_time_for_sync_end : "08:00"
