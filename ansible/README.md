# Building with jsdev user on Mac OS X using ansible

To have a somehow streamlined process on Mac OS X to work
with the build tools we implemented some ansible scripts to
unify this process.

If you have already used the corresponding ansible scripts
for the mediaCORE-UI package, you can skip most of this
and directly start [here](#Initial%20sync%20and%20install%20dependencies)

## Overview

The given ansible playbooks will setup a partly automated
testing system for your pdf.js development.

This guide assumes that you have the tool `brew` installed
on your system.

## Install ansible

Check out the documentation about how to install ansible
on your machine [here](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html).
Or you can try this:

    brew install ansible

## Install GNU Tar

To run the following playbook configuration, you need to
have GNU Tar installed:

    brew install gnu-tar

## Setup ansible playbook configuration

To use the deployment script, a new configuration file
has to be created specific to your work environment. It
has to be named `personal_variables.yml` and it requires
at least a property called `dev_source`. This property
has to be set to the absolute path of your `pdf.js` code 
directory without a trailing slash (`/`).

If your testing user is not called `jsdev`, you can modify
the standard username as well by setting property
`npm_username`.

Example `personal_variables.yml`:
```
---
dev_source: /Users/mydeveloperuser/code/pdf.js
npm_username: testinguser
```

This file contains your personal setup and should not be
committed to the repository.

## Install node for the user jsdev

To simplify the setup, you can use the install_npm.yml
ansible playbook. You can run it like this:

    sudo ansible-playbook install_npm.yml

If you have already installed the mediaCORE-UI ansible
tools you don't need to do this, since the above script
is a subset of the same script in the mediaCORE-UI tools.

## Initial sync and install dependencies

After installing nope you need to initially sync the
pdf.js code to the other user and install the
dependencies required to build pdf.js

    sudo ansible-playbook initialize.yml

## Using the build tools

By finishing the installation, all important `npm`
commands should have been taken care of apart from the
`build` command. Latter can be executed via its own
playbook:

    sudo ansible-playbook build.yml

To develop it is recommended that you run a pdf.js
server manually: 

    sudo su jsdev 
    cd ~/pdf.js
    npx gulp server

You can then load pdf.js using this url:
http://localhost:8888/web/viewer.html#file=http://localhost:8888/test.pdf

