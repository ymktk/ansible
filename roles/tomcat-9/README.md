tomcat-8
=========

Ansible role to install and configure Apache Tomcat on CentOS/RHEL.

Requirements
------------

* Tomcat supported versions by this role:
  * 8.5
* CentOS/RHEL 7
* SELinux disabled

Role Variables
--------------

- `tomcat_version`: tomcat version to install

Dependencies
------------

  - java

Example Playbook
----------------

```yaml
- hosts: servers
  remote_user: root
  become: yes
  roles:
    - tomcat-8
```

License
-------

Apache-2.0

Author Information
------------------

This role was created in 2020 by [Takahiro Yamaki](https://github.com/ymktk/).


```bash
ls -ltr /usr/lib/systemd/system/

systemctl start tomcat-i01.service
systemctl start tomcat-i02.service
```

- tomcat instance1 http://localhost:8091/
- tomcat instance2 http://localhost:8092/
