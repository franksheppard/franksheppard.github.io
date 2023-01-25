---
layout: post
title: How to use multiple Github accounts
subtitle: Setup your git workflow with multiple Github accounts on one computer
# cover-img: /assets/img/markus-winkler-ZDwh_sxsX2g-unsplash.jpg
thumbnail-img: /assets/img/roman-synkevych-wX2L8L-fGeA-unsplash-thumb.jpeg
# share-img: /assets/img/markus-winkler-ZDwh_sxsX2g-unsplash.jpg
tags: [github, ssh]
---

For my workflow I opted to create another Github account to use it for work projects. In order to juggle my work and personal Github accounts, I've set up git cli with different ssh keys. I've tried using the same ssh key for both Github accounts but the key is rejected if it has been used on another accounts. To get arround this I'm using the folwing setup. I am using a Macbook but it can work on Windows WSL though I haven't tested it.

## How I've set it up

1. Create a copy of `~/.ssh/config` and name it `config.personal` and respectively `config.work`

```bash
mv ~/.ssh/config ~/.ssh/config.personal && mv ~/.ssh/config ~/.ssh/config.work
```

2. Update the keys respectively for each `.ssh/config` work and personal

```sh
Host github.com
        HostName github.com
        User git
        IdentityFile ~/.ssh/id_rsa
```

3. edit your `~/.bashrc` or `~/.zshrc` or `~/.bash_profile` and paste in the below script.

```bash
git-switch(){
  echo "Choose your profile"
  select opt in Personal Work Quit;
  do
  case $opt in
  Personal)
  echo "Using $opt profile"
  cp ~/.ssh/config.personal ~/.ssh/config 
  break
  ;;
  Work)
  echo "Using $opt profile"
  cp ~/.ssh/config.work ~/.ssh/config
  break
  ;;
  quit)
  break
  ;;
*) 
  echo "Invalid option $REPLY"
;;
  esac
  done
}
```

04. Source your config from a terminal window

```bash
source ~/.bashrc
```

05. Now if you run `git-switch` in your terminal you should be presented with a choice of profile

```bash
Choose your profile
1) Personal  2) Work      3) Quit      
?# 
```
6. Feel free to go back and edit the script to add as many accounts you want


Lukeworm regards,
