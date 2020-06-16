# webinar

there would be a playbook to create a VM at DigitalOcean  
  
# installation   
## Ubuntu    
sudo apt update  
sudo apt install software-properties-common  
sudo apt-add-repository --yes --update ppa:ansible/ansible  
sudo apt install ansible  

# Running  
  
ansible-playbook \
- K \  
--vault-password-file ~/secret.key \
--inventory "web1, web2, " \  
--tags "check_api_health" \  
vm_create.yml  


# Webinar-memos
Copypaste lives here  
  
```
Resources:
https://github.com/Areso/Webinar-ansible
https://github.com/Areso/Webinar-memos
https://gitlab.com/Areso/simple-api

Memo1 - Ansible Installation on Ubuntu

sudo apt update  
sudo apt install software-properties-common  
sudo apt-add-repository --yes --update ppa:ansible/ansible  
sudo apt install ansible

For other distribs
https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

Create an account on Digital Ocean at https://www.digitalocean.com/ 
(you may use this referral link to get the Welcome bonus big enough for testing) https://m.do.co/c/31990fdf4722

Generate a SSH pair of keys:
cd ~/.ssh/
ssh-keygen -t rsa -b 4096
cat ~/.ssh/id_rsa.pub

On Digital Ocean:
Settings -> Security -> Add SSH Key
Settings -> Applications & API -> Get token

touch ~/secret.key
echo "YourVeryPassPhrase" > ~/secret.key 

Now, encrypt the token:
ansible-vault encrypt_string --vault-password-file ~/secret.key 'your_token_here' --name 'do_token'

replace the one from an example at group_vars/all.yml with yours

On Gitlab.com:
clone my repository https://gitlab.com/Areso/simple-api to a new

On Gitlab.com:
Settings -> Repository -> Deploy Tokens

This token should be encrypted as well:

ansible-vault encrypt_string --vault-password-file ~/secret.key 'your_token_here' --name 'gitlab_token'

replace the one from an example at group_vars/all.yml with yours
```

Create a VM:
```bash
ansible-playbook \
-k \
--vault-password-file ~/secret.key \
--inventory "web1, " \
vm_create.yml
```

Install software:
```bash
ansible-playbook \
-u root \
--vault-password-file ~/secret.key \
--inventory "web1, " \
--skip-tags "upgrade" \
software_install.yml
```
Deploy service:
```bash
ansible-playbook \
-u root \
--vault-password-file ~/secret.key \
--inventory "web1, " \
deploy_service.yml
```


Useful commands for debugging python scripts:
```bash
lsof -i -P -n
pgrep -af python
```

Update service:
```bash
ansible-playbook \
-u root \
--vault-password-file ~/secret.key \
--inventory "web1, " \
--extra-vars "raction=update_deploy" \
deploy_service.yml
```

Destroy a VM:
```bash
ansible-playbook \
-k \
--vault-password-file ~/secret.key \
--inventory "web1, " \
vm_destroy.yml
```


Destroy a VM:
```bash
ansible-playbook \
--vault-password-file ~/secret.key \
--inventory "web1, " \
vm_destroy_by_tag.yml
```
