---
layout: post
published: true
tags: [mongodb, ubuntu]
title: Configuring MongoDB on Ubuntu
---

This tutorial describes how to download and install the lastest stable version of MongoDB on an Ubuntu system.

The following instructions cover installing and upgrading MongoDB using two different methods. The first and recommended method uses apt-get to install the MongoDB package provided by the 10gen repository. If you require more control over the location of the binaries, use the instructions in "Generic Linux Binaries" to install the generic Linux binaries provided at http://www.mongodb.org/downloads.

# 10gen Repository

## Add 10gen Repository

10gen, the creator of MongoDB, publishes a MongoDB package for Ubuntu. To access the package via apt-get add the 10gen repository to /etc/apt/sources.list.

    sudo sh -c "echo 'deb http://downloads-distro.mongodb.org/repo/ubuntu-upstart dist 10gen' >> /etc/apt/sources.list.d/downloads-distro.mongodb.org.list"

Next, import the public GPG key used to sign packages contained in the 10gen repository.

    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 7F0CEB10

Run apt-get to retrieve the list of package files provided by the the new source respository.

    sudo apt-get update

## Install mongodb-10gen

Installation via the 10gen repository requires one command to download, install and automatically start MongoDB.

    sudo apt-get -y install mongodb-10gen

## Installation Script

So you've made this far through the tutorial and are thinking, "That's a lot of steps!". To simplify things I've created a shell script to automatically install MongoDB. Download and run the shell script using the following command:

    wget -q -O - https://raw.github.com/cvee/ubuntu-env/master/install-mongodb.sh | sudo sh

# Generic Linux Binaries

## Installation

    cd ~/
    git clone git://github.com/cvee/ubuntu-env.git
    sudo adduser --system --group --home /opt/mongodb --shell /bin/false --no-create-home mongodb
    sudo mkdir /opt/mongodb
    sudo mkdir /etc/mongodb
    sudo mkdir /var/opt/mongodb
    sudo mkdir /var/opt/mongodb/db
    sudo mkdir /var/opt/mongodb/log
    sudo chown -R mongodb:mongodb /var/opt/mongodb
    sudo cp ~/ubuntu-env/etc/mongodb/mongodb.conf /etc/mongodb/mongodb.conf
    sudo cp ~/ubuntu-env/etc/init/mongodb.conf /etc/init/mongodb.conf
    cd ~/
    curl -O http://fastdl.mongodb.org/linux/mongodb-linux-x86_64-2.0.1.tgz
    sudo tar -C /opt/mongodb -xzf mongodb-linux-x86_64-2.0.1.tgz
    cd /opt/mongodb
    sudo mv mongodb-linux-x86_64-2.0.1 2.0.1
    sudo ln -s 2.0.1 latest
    sudo service mongodb start

## Upgrade

    sudo service mongodb stop
    cd ~/
    curl -O http://fastdl.mongodb.org/linux/mongodb-linux-x86_64-2.0.1.tgz
    sudo tar -C /opt/mongodb -xzf mongodb-linux-x86_64-2.0.1.tgz
    cd /opt/mongodb
    sudo mv mongodb-linux-x86_64-2.0.1 2.0.1
    sudo unlink latest && sudo ln -s 2.0.1 latest
    sudo service mongodb start

# More Information

* [MongoDB](http://www.mongodb.org/)
