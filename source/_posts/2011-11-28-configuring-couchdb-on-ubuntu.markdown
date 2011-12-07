---
layout: post
published: true
tags: [couchdb, ubuntu]
title: Configuring CouchDB on Ubuntu
---

To ensure an Ubuntu system is using the lastest stable version of CouchDB currently requires building from source. This tutorial describes how to download, build, install and configure the lastest stable version of CouchDB on an Ubuntu system.

# Install Dependencies

Install the packages needed to download, build and install CouchDB.

    sudo apt-get -y install build-essential autoconf automake libtool erlang libicu-dev libmozjs-dev libcurl4-openssl-dev

# Install CouchDB

## Get the Source

Download and extract the CouchDB 1.1.1 source code.

    curl -O http://www.apache.org/dyn/closer.cgi?path=/couchdb/1.1.1/apache-couchdb-1.1.1.tar.gz
    tar -C /tmp -xzf apache-couchdb-1.1.1.tar.gz
    cd /tmp/apache-couchdb-1.1.1

OR

Clone the latest CouchDB source code:

    git clone http://git-wip-us.apache.org/repos/asf/couchdb.git /tmp/couchdb

Checkout the 1.2.x branch:
    cd /tmp/couchdb
    git checkout --track -b 1.2.x origin/1.2.x
    
Run <code>bootstrap</code> to create the <code>configure</code> script:

    ./bootstrap

## Build and Install

Build the source and install the compiled binaries.

    ./configure
    make && sudo make install

Create a user account dedicated for CouchDB:

    sudo adduser --system --home /usr/local/var/lib/couchdb --no-create-home --shell /bin/bash --group --gecos "CouchDB Administrator" couchdb

Change the ownership of the CouchDB directories:

    sudo chown -R couchdb:couchdb /usr/local/etc/couchdb
    sudo chown -R couchdb:couchdb /usr/local/var/lib/couchdb
    sudo chown -R couchdb:couchdb /usr/local/var/log/couchdb
    sudo chown -R couchdb:couchdb /usr/local/var/run/couchdb

Change the permission of the CouchDB directories:

    sudo chmod 0770 /usr/local/etc/couchdb
    sudo chmod 0770 /usr/local/var/lib/couchdb
    sudo chmod 0770 /usr/local/var/log/couchdb
    sudo chmod 0770 /usr/local/var/run/couchdb

Create a administrator, edit /usr/local/etc/couchdb/local.ini and add the section:

    [admins]
    root = <password>

To start the daemon on boot, copy the init script to /etc/init.d

    sudo cp /usr/local/etc/init.d/couchdb /etc/init.d
    sudo update-rc.d couchdb defaults

Manually start CouchDB:

    sudo service couchdb start

# More Information

* [Apache CouchDB](http://couchdb.apache.org)
