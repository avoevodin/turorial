# Test repository

## sub header

### brew

* intall brew https://brew.sh

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

* install python 3.9

```
brew search python
brew install python@3.9
```

### ssh

* generate ssh id RSA key private and public

```
ssh-keygen
```

~/.ssh/id_rsa - private part
~/.ssh/id_rsa.pub - public part (to be uploaded to github)

```
cat ~/.ssh/id_rsa.pub
```

### github

* upload public part to github https://github.com/settings/keys
* create new empty repository "saved url"

### git

* Set git global params 
```
git config --global user.name "Your Name"
git config --global user.email your@email.com

```

* create folder
```
mkdir ~/Projects/test-repository
cd ~/Projects/test-repository
```

* init repository
```
git init
```

* create file
```
touch README.md
git add README.md
```

* commit to local repository
```
git commit -m 'added README.md'

```

* push to remote repository
```
git remote add origin "saved url"
git push --set-upstream origin master

```

* git check status

```
git status

```