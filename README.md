Tomcat Ansible Role
===================

[![Build Status](https://travis-ci.com/vantaworks/ansible-role-tomcat.svg?branch=master)](https://travis-ci.com/vantaworks/ansible-role-tomcat)

Ansible role to install, configure, and update Apache's Tomcat on Linux. This role tracks the versions tagged in [Tomcat's GitHub repo](https://github.com/apache/tomcat) and then downloads Tomcat from the Apache Foundation mirrors. Those looking for an Ansible role that updates Tomcat every run, then this is for you.

Requirements
------------
Requires at least Java 8. Below are two viable options:

1. [`geerlingguy.java`](https://galaxy.ansible.com/geerlingguy/java)
2. [`ansiblebit.oracle-java`](https://galaxy.ansible.com/ansiblebit/oracle-java)

Role Variables
--------------
Available variables are listed below, along with default values (see `defaults/main.yml`):

	tomcat_major_version: 8

The major version to use when installing Tomcat.

	tomcat_minor_version: 8.5.57
	# default is undefined

A specific Tomcat minor version to lock to. (Recommendation: leave undefined so role will download latest revision of specified major version).

	tomcat_mirror: "http://apache.mirrors.hoobly.com"

Which Apache Foundation mirror to download Tomcat from.

	tomcat_user: tomcat
	tomcat_group: tomcat

	tomcat_user_uid: ""
	tomcat_group_gid: ""
	# default is undefined

Specify the Tomcat service account parameters, including GID/UID (optional).

	tomcat_service_name: "tomcat"

Name of the system service.

	tomcat_service_enabled: True

Whether or not to enable the Tomcat service.

	tomcat_jvm_percentage_xms: 15
	tomcat_jvm_percentage_xmx: 55

JVM memory allocation percentages.

	tomcat_debug_mode: False

Whether or not to enable JMX debugging for Tomcat.


Dependencies
------------
No Ansible-Python dependencies. See [Requirements](#requirements) above for role requirements.

Example Playbook
----------------
```
# Install and maintain Tomcat 8 (currently 8.5)
- name: Example Install Play 1 - Production
  hosts: tomcat
  vars:
    tomcat_major_version: 8
  roles:
    - vantaworks.tomcat

- name: Example Install Play 2 - Development
  hosts: tomcat_dev
  vars:
    tomcat_major_version: 9
    tomcat_permissions_production: False
    tomcat_users:
      - username: "tomcat"
        password: "lamepassword"
        roles: "tomcat,admin,manager,manager-gui"
      - username: "developer"
        password: "worsepw"
        roles: "tomcat,admin,manager,manager-gui"
  roles:
    - vantaworks.tomcat

- name: Example Uninstall Play
  hosts: tomcat
  vars:
    tomcat_state: "absent"
    tomcat_uninstall_create_backup: True
    tomcat_uninstall_remove_user: True
    tomcat_uninstall_remove_group: True
    tomcat_uninstall_remove_all: True
  roles:
    - vantaworks.tomcat
```

License
-------

BSD
