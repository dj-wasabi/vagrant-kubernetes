# Vagrant Kubernetes

Table of Content
  * [Requirements](#requirements)
  * [Setup](#setup)
  * [Kubernetes version](#kubernetes-version)
  * [Starting](#starting)
  * [Credits](#credits)

A small playground to experiment or play with Kubernetes on multiple Vagrant Ubuntu `ubuntu/bionic64` instances. So do not use this as a base for production like deployments (Kubespray for example).

## Requirements

Please make sure the following is installed before using this repo:

* Ansible;
* Vagrant;
* VirtualBox;

## Setup

Default the following is started/installed:

* 1 Control node;
* 2 Worker nodes;

Each node has 2 CPU's configured with each 2GB of RAM. You can change this to your needs by updating the `Vagrantfile`.

## Kubernetes version

Kubernetes version 1.19.3 which can be changed in the `Vagrantfile` by looking for the `k8s_version` in the `ansible.extra_vars` property.

```ruby
    ansible.extra_vars = {
        ...
        k8s_version: "1.19.3",
        ansible_python_interpreter: "/usr/bin/python3",
    }
```

## Starting

Once you are ready, run the following to start everything:

```sh
vagrant up
```

Once booted, use the following command to logon to the control node:

```sh
vagrant ssh control
```

You should be able to run `kubectl get nodes` now.

## Credits

Combination of code https://graspingtech.com/create-kubernetes-cluster/ and some custom things.
