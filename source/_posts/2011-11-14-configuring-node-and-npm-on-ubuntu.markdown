---
layout: post
published: true
tags: [node.js, npm, ubuntu]
title: Configuring node.js and npm on Ubuntu
---

The rapid pace of development on node.js and npm has resulted in frequent releases and huge improvements. As an unfortunate side-effect, the official Ubuntu provided packages quickly become outdated. To ensure an Ubuntu system is using the lastest stable (or unstable) versions of node.js and npm currently requires an extra bit of work. This tutorial describes how to install the lastest stable versions of node.js and npm on an Ubuntu system.

## Install Developer Packages

Install the packages needed to download, build and install node.js and npm.

    sudo apt-get -y install build-essential libssl-dev openssh-server pkg-config curl git-core

## node.js Installation

    curl -L http://nodejs.org/dist/v0.6.6/node-v0.6.6.tar.gz | tar -C /tmp -xzf -
    cd /tmp/node-v0.6.6
    ./configure
    make -j2 && sudo make install
    cd ~/
    rm -rf /tmp/node-v0.6.5

## npm Installation

As of node.js 0.6.3, npm is included and installed on make install. The following installation instructions are included for historical purposes only.

    curl http://npmjs.org/install.sh | sudo sh

## Create a Dedicated User Account

To avoid the security concerns caused by running applications as root, you may want to create a user responsible for running node apps.

    sudo adduser --home /home/node --shell /bin/bash --disabled-password node

Become the user and generate an SSH keypair:

    sudo su - node
    ssh-keygen -b 4096 -t rsa

For security, the 'node' user account is created with password logins disabled. In order to login via SSH as 'node', create or edit the file /home/node/.ssh/authorized_keys then copy in your public key. Save /home/node/.ssh/authorized_keys and assign it the correct access permissions.

    chmod 600 /home/node/.ssh/authorized_keys

## Installation Script

To automate installation I've collected the above steps into a shell script. To install node.js and npm using this method download and run the shell script using the following command:

    wget -q -O - https://raw.github.com/cvee/ubuntu-env/master/install-node.sh | sudo sh

## More Information

* [node.js](http://nodejs.org/)
* [npm](http://npmjs.org/)
