# How to start

```bash

cd ~

source ansible-py3/bin/activate

ansible --version


# Syntax check
ansible-playbook -i inventory playbook.yml --syntax-check

# Task list
ansible-playbook -i inventory playbook.yml --list-tasks

# Run
# -vvv 詳細情報の表示及び、実行結果をJSONで返す
ansible-playbook -i inventory playbook.yml -vvv

```

# Links

- (Module)[https://docs.ansible.com/ansible/latest/modules/modules_by_category.html]
- (Ansible Galaxy)[https://galaxy.ansible.com/]

# References

- (Ansibleでできることを中の人が教えます - インストールと実行〜EC2へのNginx投入までを学ぼう)[https://employment.en-japan.com/engineerhub/entry/2019/04/12/103000]


| | Ansible core | Ansible Management tool | Support |
| ---- | ---- | ---- | ---- |
| Community  | Ansible Project |  AWX  |  No  |
| Commercial | Ansible Engine  |  Ansible Tower  |  Yes  |
