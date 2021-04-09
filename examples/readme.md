# Examples

This directory contains several example questions you can do to get yourself either familiar with Kubernetes and/or maybe even help you prepare for the CKA(D) Exams. And these examples are by no means enough for successfully completing CKA(D) examns, just to get you started. You still need to work (almost) daily with Kubernetes and dreaming about everthing K8s related. :).

[Work in Progress]

## CKA

* [Cluster Architecture, Installation & Configuration](01_cluster.md)
* [Workloads & Scheduling](02_scheduling.md)
* [Services & Networking](03_networking.md)
* [Storage](04_storage.md)
* [Troubleshooting](05_troubleshooting.md)

## CKAD

* [Core Concepts](11_core.md)
* [Configuration](12_configuration.md)
* [Multi-Container Pods](13_multipods.md)
* [Observability](14_observability.md)
* [Pod Design](15_poddesign.md)
* [Services & Networking](16_services_networking.md)
* [State Persistence](17_state_persistence.md)

# Tips

## Completion

Make sure you can use bash completion to work for `kubectl` command. You'll have to do the following:

```
kubectl completion bash >> ~/.bash_profile
source ~/.bash_profile
```

## .vimrc

There are some questions that you'll need to create or edit files. It might help to create a `.vimrc` file with some default configuration.

```
vim ~/.vimrc
```
Add the following contents:

```
set ts=2 sts=2 sw=2 et nu paste
```

|Option|Description|
|---|---|
|`ts`| Number of spaces that <Tab> in file uses.|
|`sts`| Number of spaces that <Tab> uses while editing.|
|`sw`| Number of spaces to use for (auto)indent step.|
|`et`| Use spaces when <Tab> is inserted.|
|`nu`| Print the line number in front of each line.|
|`paste`| Allow pasting text.|

(See [this](http://vimdoc.sourceforge.net/htmldoc/quickref.html#option-list) page for more options)

Most of the things is to prevent having YAML issues. :)
