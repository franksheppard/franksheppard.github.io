---
layout: post
title: Configure SSH key
subtitle: Setup your git workflow to work with ssh 
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [git, ssh]
---


1. Create your key either for a speciffic emai address or for your use
`ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`

`ssh-keygen -t rsa -b 4096 -C $USER`

2. Add the key to ssh

`ssh-add ~/.ssh/id_rsa`

3. The keys are usually saved in  
`ls ~/.ssh` the default key name is `id_rsa` for private key and for the public  `id_rsa.pub`
