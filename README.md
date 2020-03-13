# 1. How to get necessary Docker image

```bash
git clone https://github.com/ymktk/docker-k8s.git

# 1-1. Create the ansible controller image
cd /path/to/docker-k8s/ansible/
docker-compose build
# -> ansible_controller is created

# 1-2. Create the target server image
cd /path/to/docker-k8s/centos-systemd/
docker-compose build
# -> centos-systemd_server is created
```

# 2. Add a public key into a target server

```bash
# 2-1. Start the target server
docker run -d --name target centos-systemd_server

# 2-2. Extract the public key from the ansible controller
export TMPDIR=/c/Users/Public/docker-shared
docker run -it --rm ansible_controller cat /home/ansible/.ssh/id_rsa.pub >> $TMPDIR/tmp-id_rsa.pub

# 2-3. Add the public key into the target server and create modified image
docker container cp $TMPDIR/tmp-id_rsa.pub target:/root/.ssh/authorized_keys
docker container cp $TMPDIR/tmp-id_rsa.pub target:/home/jenkins/.ssh/authorized_keys
docker container cp $TMPDIR/tmp-id_rsa.pub target:/home/tomcat/.ssh/authorized_keys

docker exec -it -u root target bash

chown root:root /root/.ssh/authorized_keys
chmod 600       /root/.ssh/authorized_keys

chown jenkins:jenkins /home/jenkins/.ssh/authorized_keys
chmod 600             /home/jenkins/.ssh/authorized_keys

chown tomcat:tomcat /home/tomcat/.ssh/authorized_keys
chmod 600           /home/tomcat/.ssh/authorized_keys

ls -ld /root/.ssh  /home/jenkins/.ssh  /home/tomcat/.ssh
ls -l  /root/.ssh/ /home/jenkins/.ssh/ /home/tomcat/.ssh/

exit

docker container stop target
docker commit target centos-systemd_server
docker container rm   target
```

# 3. How to start test servers

```bash
cd /path/to/ansible/

docker-compose up -d

# 3-1. Ansible controller
docker exec -it con sh
ansible --version
python3 -V
pip3 -V

## ssh test
ssh root@jk
exit

ssh jenkins@jk
sudo -l
exit

## Ansible play
cd ~/code/roles/

# Ping test
ansible -i inventory cicd_servers -m ping -u root -vvv



# Jenkins
#   Install Master wo plugins
ansible-playbook -i inventory playbook-build-jenkins.yml --list-tasks
ansible-playbook -i inventory playbook-build-jenkins.yml -vv

#   Install Jenkins plugins
ansible-playbook -i inventory playbook-build-jenkins.yml --tags "plugins" --list-tasks
ansible-playbook -i inventory playbook-build-jenkins.yml --tags "plugins" -vv

# tomcat 9
#   Unarchive tomcat.tar.gz + Setup tomcat (common)
ansible-playbook -i inventory playbook-build-tomcat-install.yml --list-tasks
ansible-playbook -i inventory playbook-build-tomcat-install.yml -vv

# Start Jenkins
docker exec -it jk bash
systemctl start  jenkins2
systemctl status jenkins2


#   Setup tomcat (Each instance)
ansible-playbook -i inventory playbook-build-tomcat.yml --tags "instances" --list-tasks
ansible-playbook -i inventory playbook-build-tomcat.yml --tags "instances" -vv



# 3-2. Target server
docker exec -it app bash

ls -ltr /usr/lib/systemd/system/

systemctl start tomcat-i01.service
systemctl start tomcat-i02.service

```

# Applications

    - http://localhost:8083/jenkins/
    - http://localhost:8081/artifactory/
    - tomcat instance1 http://localhost:8091/
    - tomcat instance2 http://localhost:8092/

# Links

- (Module)[https://docs.ansible.com/ansible/latest/modules/modules_by_category.html]
- (Ansible Galaxy)[https://galaxy.ansible.com/]

# References

- Ansible
    - (Ansibleでできることを中の人が教えます - インストールと実行〜EC2へのNginx投入までを学ぼう)[https://employment.en-japan.com/engineerhub/entry/2019/04/12/103000]
- Jenkins
    - (geerlingguy/ansible-role-jenkins)[https://github.com/geerlingguy/ansible-role-jenkins]
- Tomcat
    - (zaxos/tomcat-ansible-role)[https://github.com/zaxos/tomcat-ansible-role]
    - (Tomcat の初期設定まとめ)[https://qiita.com/hidekatsu-izuno/items/ab604b6c764b5b5a86ed]
- MS SQL server
    - (docker hub Microsoft SQL Server)[https://hub.docker.com/_/microsoft-mssql-server]
- Artifactory OSS
    - https://www.jfrog.com/confluence/display/RTF5X/Installing+with+Docker
    - (How do I login to Artifactory for the first time? What are the default admin username and password?)[https://www.jfrog.com/confluence/display/RT12/FAQs#FAQs-HowdoIlogintoArtifactoryforthefirsttime?Whatarethedefaultadminusernameandpassword?]


| | Ansible core | Ansible Management tool | Support |
| ---- | ---- | ---- | ---- |
| Community  | Ansible Project |  AWX  |  No  |
| Commercial | Ansible Engine  |  Ansible Tower  |  Yes  |
