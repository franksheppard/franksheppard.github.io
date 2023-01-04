---
layout: post
title: Run different versions of Python
subtitle: Setup your Mac to work with multiple versions of Python 
# cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
# share-img: /assets/img/path.jpg
tags: [python, mac]
published: true
---
# Create Python virtual environments 

While the software industry matures a number of release , versions are available.
If you want to use multiple versions of Python on the same computer you can use Pyenv. In combination with virtualenv you can create a dynamic local Python development environment
## Selecting the Python version you want.
1. Install pyenv
2. Use `pyenv install -l` to see the available versions
3. Install the version you want
   
    `pyenv install 3.6.15`

## Create day-to-day virtual environments.

1. Install `virtualenv`
2. create the virtual environment
   
    `pyenv virtualenv 2.7.16 apps2`


## Activate virtual environments locally

1. Create the environment

    `pyenv virtualenv 3.7.4 rst2pdf-py3`
1. Use pyenv local withing directory
    ```bash
    cd ~/dev/rst2pdf
    pyenv local rst2pdf-py3
    ```