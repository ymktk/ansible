# 3. How to start test servers

```bash
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

| | Ansible core | Ansible Management tool | Support |
| ---- | ---- | ---- | ---- |
| Community  | Ansible Project |  AWX  |  No  |
| Commercial | Ansible Engine  |  Ansible Tower  |  Yes  |
