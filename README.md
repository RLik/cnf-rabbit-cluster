# Example of configuring RabbitMQ cluster server from box by GitLab pipeline

I use this Ansile playbookas a base to configuring RabbitMQ clister out of box for my labs using GitLab pipeline. Playbook was tested on followins OS: Ubuntu Server 22.04.

You can freely use this solution, but without any guarantee from the author.

## Environment description

I use GitLab CE server with local shell agent and Ansible installed. GitLab runner run under gitlab-runner user account.

## Step to prepare

- [ ] Switch to gitlab-runner user context:
```
sudo su - gitlab-runner
```
- [ ] Generate SSH keys if they wasn't generated early:
```Updates were rejected because a pushed branch tip is behind its remote
ssh-keygen
```
- [ ] Correct inventory file (inventory/inv.ini) and add appropriate hosts.
- [ ] Copy SSH public key to all servers form inventory file:
```
ssh-copy-id roman@10.10.10.31
ssh-copy-id roman@10.10.10.30
ssh-copy-id roman@10.10.10.29
```
- [ ] Add sudo password for all hosts.
```
sudo nano /etc/ansible/vault/rabbitmq.yml
```
Format is (replace passwort to yours):
```
ansible_sudo_pass: P@$$word
```
- [ ] Encrypt file with sudo password by Ansible Vault:
```
sudo ansible-vault encrypt /etc/ansible/vault/rabbitmq.yml
```
- [ ] Give rights to read sudo password file only to root and gitlab-runner users:
```
sudo chown root:gitlab-runner /etc/ansible/vault/rabbitmq.yml
sudo chmod 640 /etc/ansible/vault/rabbitmq.yml
sudo chmod 650 /etc/ansible/vault/
```

- [ ] In GitLab Project settings add variale named $ANSIBLE_VAULT_PASSWORD and in value part specify password that you've use to encrypt galera.yml file by Ansible Vault.