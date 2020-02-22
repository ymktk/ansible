Role Name
=========

Installs Jenkins for RedHat/CentOS servers.

Requirements
------------

No.

Role Variables
--------------

Available variables are listed below. Default values are here `defaults/main.yml`.


    jenkins_version: "2.221-1.1"

Jenkins version can be pinned to any version available on http://pkg.jenkins-ci.org/redhat/ .

    jenkins_home: /data/jenkins

The Jenkins home directory is being used for storing data. (Default `/var/lib/jenkins`)

    jenkins_http_port: 8082

The HTTP port for Jenkins' web interface.

    jenkins_admin_username: admin
    jenkins_admin_password: admin

Default admin account credentials which will be created the first time Jenkins is installed.

    jenkins_jar_location: /opt/jenkins-cli.jar

The location at which the `jenkins-cli.jar` will be kept.

    jenkins_url_prefix: "/jenkins"

Used for setting a URL prefix for your Jenkins installation. The option is added as `--prefix={{ jenkins_url_prefix }}` to the Jenkins initialization `java` invocation, so you can access the installation at a path like `http://www.example.com{{ jenkins_url_prefix }}`.

    jenkins_init_changes:
      - option: "JENKINS_ARGS"
        value: "--prefix={{ jenkins_url_prefix }}"
      - option: "JENKINS_JAVA_OPTIONS"
        value: "-Djenkins.install.runSetupWizard=false -Dorg.apache.commons.jelly.tags.fmt.timeZone=Asia/Singapore"

Changes made to the Jenkins init script; the default set of changes set the configured URL prefix and add in configured Java options for Jenkins' startup. You can add other option/value pairs if you need to set other options for the Jenkins init file.

    jenkins_proxy_host: ""
    jenkins_proxy_port: ""
    jenkins_proxy_noproxy:
      - "127.0.0.1"
      - "localhost"

Proxy settings


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
    - jenkins-master
```

License
-------

Apache-2.0

Author Information
------------------

This role was created in 2020 by [Takahiro Yamaki](https://github.com/ymktk/).
