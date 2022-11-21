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
or
```
git clone https://github.com/vichanzo/Ansible-pull-and-Github.git
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
