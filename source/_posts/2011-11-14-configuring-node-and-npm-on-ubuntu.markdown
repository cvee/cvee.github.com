---
layout: post
published: true
tags: [mongodb, node.js, npm, ubuntu]
title: Configuring node.js, npm and mongoDB on Ubuntu
---

The rapid pace of development on node.js, npm and mongoDB has resulted in frequent releases and huge improvements. As an unfortunate side-effect, the official Ubuntu provided node.js, npm and mongoDB packages quickly become outdated. To ensure an Ubuntu system is using the lastest stable (or unstable) versions of node.js, npm and mongDB currently requires an extra bit of work. This tutorial describes how to install the lastest stable versions of node.js, npm and mongoDB on an Ubuntu system.

## Install Developer Packages

Install the packages needed to download, build and install node.js and npm.

    sudo apt-get -y install build-essential libssl-dev openssh-server pkg-config curl git-core

## node.js Installation

    cd ~/
    curl -O http://nodejs.org/dist/v0.6.3/node-v0.6.3.tar.gz
    tar -C /tmp -xzf node-v0.6.3.tar.gz
    cd /tmp/node-v0.6.3
    ./configure
    make -j2 && sudo make install
    cd ~/
    rm -rf /tmp/node-v0.6.3

## npm Installation

As of node.js 0.6.3, npm is included in packages/installers and installed on make install. The following installation instructions are included for historical purposes only.

    curl http://npmjs.org/install.sh | sudo sh

## Short Cut

So you've made this far through the tutorial and are thinking, "That's a lot of steps!". To simplify things I've created a shell script to automatically install mongoDB, node.js and npm. Download and run the shell script using the following command:

    wget -q -O - https://raw.github.com/cvee/ubuntu-env/master/install.sh | sudo sh

## Create a Dedicated User Account

To avoid the security concerns caused by running applications as root, you may want to create a user responsible for running node apps.

    sudo adduser --home /home/node --shell /bin/bash --disabled-password node
    sudo mkdir /home/node/.ssh
    sudo chown node:node /home/node/.ssh
    sudo chmod 700 /home/node/.ssh
    
For security, the 'node' user account is created with password logins disabled. In order to login via SSH as 'node', creat or edit the file /home/node/.ssh/authorized_keys then copy in your public key. Save /home/node/.ssh/authorized_keys and assign it the correct access permissions.

    sudo chown node:node /home/node/.ssh/authorized_keys
    sudo chmod 600 /home/node/.ssh/authorized_keys

# More Information

* [node.js](http://nodejs.org/)
* [npm](http://npmjs.org/)
