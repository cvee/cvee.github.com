---
layout: post
published: false
tags: [couchdb, node.js, npm, ubuntu]
title: Configuring a Private npm Registry
---

To ensure an Ubuntu system is using the lastest stable version of CouchDB currently requires building from source. This tutorial describes how to download, build, install and configure the lastest stable version of CouchDB on an Ubuntu system.

Some of these projects may or may not be ready for public consumption and you'd like to try publishing to the npm registry. Fortunately the creators of npm have made it possible to create your own private registry.


# Prerequisites

As of this document, the npm registry is approximately 6.2GB so make sure you have enough storage space on your server before attempting to replicate.

## Install Git

Some of the soure code needed is available via GitHub. To access this code we'll need to install <code>git</code> via <code>apt-get</code>: 

    sudo apt-get -y install git-core

## Install node.js and npm

See [Configuring node.js, npm and mongoDB on Ubuntu](http://cvee.github.com/2011/11/14/configuring-node-npm-and-mongodb-on-ubuntu.html).

## Install CouchDB

In order to provide access to public packages in addition to our private ones, our private registry will need to replicate the official npm database. npm uses CouchDB as its data store therefore we must setup an internal CouchDB on our server. See [Configuring CouchDB on Ubuntu](http://cvee.github.com/2011/11/18/configuring-couchdb-on-ubuntu.html) for information on how to get CouchDB installed and running.

Note: Due to a bug in CounchDB 1.1.1 ([COUCHDB-1327](https://issues.apache.org/jira/browse/COUCHDB-1327)) attempts to use replication will result in <code>{"error":"json_encode","reason":"{bad_term,{nocatch,{invalid_json,<<>>}}}"}</code>. To successfully replicate, CouchDB must be built from the 1.2.x source code branch.

# Replicate the CouchDB Database

Install the couchapp module:

    sudo npm install -g couchapp

Create a user account dedicated for npm:

    sudo adduser --system --home /home/npm --shell /bin/bash --group --gecos "npm Administrator" npm

Change to npm user:

    sudo su - npm

Created a directory structure for locally installed node modules

    mkdir ~/local && mkdir ~/local/lib && mkdir ~/local/lib/node_modules

Clone the npmjs module:

    cd ~/local/lib/node_modules
    git clone https://github.com/isaacs/npmjs.org.git
    cd npmjs.org
    npm install -d

Create a user for the registry database, edit /usr/local/etc/couchdb/local.ini and add the section:

    [admins]
    registry = <password>

Create a new database for the registry data:

    curl -X PUT http://registry:<password>@localhost:5984/registry

Synchorize the registry and search objects

    couchapp push registry/app.js http://registry:<password>@localhost:5984/registry
    couchapp push www/app.js http://registry:<password>@localhost:5984/registry

Replicate the public registry:

    curl -H 'Content-Type: application/json' -X POST http://registry:<password>@localhost:5984/_replicate -d '{"source": "http://isaacs.couchone.com/registry", "target": "registry"}'

# More Information

* [Apache CouchDB](http://couchdb.apache.org)
* [node.js](http://nodejs.org/)
* [npm](http://npmjs.org/)
