# Vagrant Kubernetes

Table of Content
  * [Requirements](#requirements)
  * [Setup](#setup)
  * [Kubernetes version](#kubernetes-version)
  * [Starting](#starting)
  * [Examples](#examples)
  * [Credits](#credits)

A small playground to experiment or play with Kubernetes on multiple Vagrant Ubuntu `ubuntu/bionic64` instances. So do not use this as a base for production like deployments (Kubespray for example).

## Requirements

Please make sure the following is installed before using this repo:

* Ansible;
* Vagrant;
* VirtualBox;
* Motivation!

## Setup

Default the following is started/installed:

* 1 Control node;
* 2 Worker nodes;

Each node has 2 CPU's configured with each 2GB of RAM. You can change this to your needs by updating the `Vagrantfile`.

## Kubernetes version

Kubernetes version 1.25.0 which can be changed in the `Vagrantfile` by looking in the top of the file for the line that starts with: `K8S_VERSION`. You can set that to a more recent version of Kubernetes before you start everything. But you can also keep the current version and upgrade to a newer version, which is also part of the CKA exam. ;-)

```ruby
    IMAGE_NAME = "ubuntu/bionic64"
    K8S_VERSION = "1.25.0"
    N = 2
```

## Starting

Once you are ready, run the following to start everything:

```sh
vagrant up
```

Once everything is booted, use the following command to logon to the control node:

```sh
vagrant ssh control
```

You should be able to run `kubectl get nodes` now.

## Examples

In the `examples` [directory](examples/readme.md), you can find some example questions that you can use to get familiar with Kubernetes.

## Credits

Combination of code https://graspingtech.com/create-kubernetes-cluster/ and some custom things.
