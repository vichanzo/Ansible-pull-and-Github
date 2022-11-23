# Ansible-pull-and-Github
Following a youtube guide on Ansible-Pull and Github

This github was created follwing this guide by Learn Linux TV
https://www.youtube.com/watch?v=sn1HQq_GFNE

Create a new ssh key:
```
ssh-keygen
```

In Github click on your account, go to "settings > SSH and GPG Keys > New SSH Key"

Now clone this respository to your system:
```
cd ~
git clone git@github.com:vichanzo/Ansible-pull-and-Github.git
```

If needed configure your name and e-mail (esc + :wq to save the file).
```
git config --global --edit
```

Create a file with "nano test-commit.txt" then push it to git.
```
git status
git add test-commit.txt
git commit -m "first commit"
git push origin main
```

## Create Ansible playbook
Edit a file called local.yml
```
nano local.yml
```
```
- hosts: localhost
  connection: local
  become: true
  
  tasks:
  - name: Install htop
    dnf:
      name: htop
```

```
git status
git add local.yml
git commit -m "initial commit of local.yml - install htop"
git push origin main
```

## Running ansible-pull
On a different system you can run the local.yml playbook

```
sudo ansible-pull -U https://github.com/vichanzo/Ansible-pull-and-Github.git
```

## Creating files ansible sudoers
Create a folder called "files" and a file sudoers_ansible
```
cd ~/Ansible-pull-and-Github
mkdir files
cd files
echo "ansible ALL=(ALL) NOPASSWD: ALL" > sudoers_ansible
```

Create a new file:
```
cd ~/Ansible-pull-and-Github
nano users.yml
```
The content of user.yml:
```
- name: create ansible user
  user:
    name: ansible
    system: yes

- name: copy sudoers_ansible
  copy:
    src: files/sudoers_ansible
    dest: /etc/sudoers.d/ansible
    owner: root
    group: root
    mode: 0440
```

## Setting up cron job
By setting up a cron job you can have the host automatically check GitHub for changes to your playbook
when changes are detected it will run/re-run the playbook with any new changes.

Create a file called cron.yml:
```
cd ~/Ansible-pull-and-Github
nano cron.yml
```
The content of cron.yml:
```
- name: install cron job (ansible-pull)
  cron:
    user: ansible
    name: "ansible provision"
    minute: "*/10"
    job: "/usr/bin/ansible-pull -o -U https://github.com/jlacroix82/ansible_pull_tutorial.git > /dev/null"
```
    
