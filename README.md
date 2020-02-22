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

docker exec -it target bash

chown root:root /root/.ssh/authorized_keys
chmod 600       /root/.ssh/authorized_keys

chown jenkins:jenkins /home/jenkins/.ssh/authorized_keys
chmod 600             /home/jenkins/.ssh/authorized_keys

ls -ld /root/.ssh  /home/jenkins/.ssh
ls -l  /root/.ssh/ /home/jenkins/.ssh/

docker container stop target
docker commit target centos-systemd_server
docker container rm   target
```

# 3. How to start test servers

```bash
cd /path/to/ansible/

docker-compose up -d

# 3-1. Ansible controller
docker exec -it con bash
ansible --version
python3 -V
pip3 -V

## ssh test
ssh root@app
exit
ssh jenkins@app
exit

## Ansible play
cd ~/code/roles/

# Syntax check
ansible-playbook -i java/tests/inventory java/tests/playbook.yml --syntax-check -vvv

# Task list
ansible-playbook -i java/tests/inventory java/tests/playbook.yml --list-tasks -vvv

# Ping test
ansible all -i java/tests/inventory -m ping -u root -vvv

# Run
# -vvv 詳細情報の表示及び、実行結果をJSONで返す
ansible-playbook -i java/tests/inventory java/tests/playbook.yml -vvv


# 3-2. Target server
docker exec -it app bash
java --version

```

# Links

- (Module)[https://docs.ansible.com/ansible/latest/modules/modules_by_category.html]
- (Ansible Galaxy)[https://galaxy.ansible.com/]

# References

- (Ansibleでできることを中の人が教えます - インストールと実行〜EC2へのNginx投入までを学ぼう)[https://employment.en-japan.com/engineerhub/entry/2019/04/12/103000]
- (docker hub Microsoft SQL Server)[https://hub.docker.com/_/microsoft-mssql-server]
- (geerlingguy/ansible-role-jenkins)[https://github.com/geerlingguy/ansible-role-jenkins]

| | Ansible core | Ansible Management tool | Support |
| ---- | ---- | ---- | ---- |
| Community  | Ansible Project |  AWX  |  No  |
| Commercial | Ansible Engine  |  Ansible Tower  |  Yes  |
