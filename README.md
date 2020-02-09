# How to start

```bash

cd ~

source ansible-py3/bin/activate

ansible --version


# Syntax check
ansible-playbook -i inventory playbook.yml --syntax-check

# Run
ansible-playbook -i inventory playbook.yml

```

# References

- (Ansibleでできることを中の人が教えます - インストールと実行〜EC2へのNginx投入までを学ぼう)[https://employment.en-japan.com/engineerhub/entry/2019/04/12/103000]
