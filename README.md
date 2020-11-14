# Replicated Couchbase Cluster with Kubernetes

Create a replicated Couchbase cluster with Kubernetes

## Motivation

Create a replicated [Couchbase](http://www.couchbase.com) cluster (which is a minimum of 3 nodes).

I have previously tried out a [Replicated Cassandra Database](http://github.com/mramshaw/Kubernetes/tree/master/Replicated%20Cassandra%20Database)
as well as a [RESTful Couchbase](http://github.com/mramshaw/RESTful-Couchbase) exercise. Here we
are using Kubernetes to set up a scalable replicated Couchbase cluster.

> The Couchbase Autonomous Operator provides native integration of Couchbase Server with open source Kubernetes and Red Hat OpenShift.

From:

    http://docs.couchbase.com/operator/current/overview.html

So, that's the following platforms:

* Kubernetes
* Red Hat Openshift
* Amazon EKS
* Google GKE
* Microsoft AKS

This exercise follows on from my [Replicated Cassandra Database](http://github.com/mramshaw/Kubernetes/tree/master/Replicated%20Cassandra%20Database)
exercise.

For an example of accessing Couchbase with Golang, see my [RESTful-Couchbase](http://github.com/mramshaw/RESTful-Couchbase) repo.

## Contents

The contents are as follows:

* [Prerequisites](#prerequisites)
* [Method](#Method)
* [Preparation](#preparation)
    * [Increase minikube's working memory](#increase-minikubes-working-memory)
    * [Increase minikube's processors](#increase-minikubes-processors)
    * [Restart minikube](#restart-minikube)
    * [Verify minikube's config](#verify-minikubes-config)
* [Testing](#testing)
    * [Namespace](#namespace)
* [Teardown](#teardown)
* [Reference](#reference)
    * [Kubernetes namespaces](#kubernetes-namespaces)
    * [Couchbase Operator Architecture](#couchbase-operator-architecture)
    * [Guidelines and Best Practices](#guidelines-and-best-practices)
    * [Creating TLS Certificates](#creating-tls-certificates)
* [Versions](#versions)
* [To Do](#to-do)
* [Credits](#credits)

## Prerequisites

* __kubectl__ installed.
* __minikube__ installed.

## Method

There are generally three ways of operating in the cloud:

1. Locally with __minikube__
2. In the cloud, using a dashboard
3. Locally as well as in the cloud, using command-line tools

In this document we will use the first option (minikube).

## Preparation

The following steps may not be absolutely necessary, but will probably save time and aggravation.

Couchbase is a ___beast___, so first we should increase minikube's limits.

#### Increase minikube's working memory

Allocate lots of virtual memory:

```bash
$ minikube config set memory 8192
❗  These changes will take effect upon a minikube delete and then a minikube start
$
```

[The default virtual memory is 3900 MB.]

#### Increase minikube's processors

Allocate lots of virtual processors:

```bash
$ minikube config set cpus 4
❗  These changes will take effect upon a minikube delete and then a minikube start
$
```

[The default number of virtual CPUs is 2.]

#### Restart minikube

Activate the new configuration options as follows:

```bash
$ minikube delete && minikube start
🔥  Deleting "minikube" in docker ...
🔥  Deleting container "minikube" ...
🔥  Removing /home/martin/.minikube/machines/minikube ...
💀  Removed all traces of the "minikube" cluster.
😄  minikube v1.14.2 on Ubuntu 18.04
✨  Automatically selected the docker driver
👍  Starting control plane node minikube in cluster minikube
🔥  Creating docker container (CPUs=4, Memory=8192MB) ...
🐳  Preparing Kubernetes v1.19.2 on Docker 19.03.8 ...
🔎  Verifying Kubernetes components...
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" by default
$
```

#### Verify minikube's config

Verify minikube's config as follows:

```bash
$ minikube config view
- memory: 8192
- cpus: 4
$
```

## Testing

#### Namespace

Create a namespace as follows:

```bash
$ kubectl create namespace couchbase
namespace/couchbase created
$
```

This will enable us to more clearly follow what is happening in the Kubernetes console:

```bash
$ kubectl get namespace
NAME              STATUS   AGE
couchbase         Active   2m18s
default           Active   4m50s
kube-node-lease   Active   4m51s
kube-public       Active   4m51s
kube-system       Active   4m51s
$
```

[We will be able to subset upon the `couchbase` namespace with either `kubectl` or the
 Kubernetes console.]

## Teardown

Delete the namespace as follows:

```bash
$ kubectl delete namespace couchbase
namespace/couchbase deleted
$
```

Finally, stop minikube:

```bash
$ minikube stop
✋  Stopping node "minikube"  ...
🛑  1 nodes stopped.
$
```

## Reference

Some useful references follow.

#### Kubernetes namespaces

    http://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/

#### Couchbase Operator Architecture

    http://docs.couchbase.com/operator/current/concept-operator.html

#### Guidelines and Best Practices

    http://docs.couchbase.com/operator/current/best-practices.html

[Reading this is a __must__, it covers many useful things.]

#### Creating TLS Certificates

    http://docs.couchbase.com/operator/current/tutorial-tls.html

## Versions

* Docker __19.03.8__
* kubectl (Client: __v1.19.3__, Server: __v1.19.2__)
* kubernetes __v1.19.2__
* minikube __v1.15.0__

## To Do

- [ ] Fix Kubernetes warnings
- [ ] Investigate deploying [Couchbase on Amazon EKS](https://blog.couchbase.com/deploy-self-healing-highly-available-couchbase-cluster-on-kubernetes-using-persistent-volumes/)

## Credits

Sadly, internet content often expires. So I generally like to include all needed files into my repos,
so as to guard against losing access to some needed reource. Even so, I always like to give credit.

The following links refer to the original sources for files included in this repo (kudos to author
Ram Dhakne for making them available).

YAML files and binaries:

    http://packages.couchbase.com/kubernetes/1.2.0/couchbase-autonomous-operator-kubernetes_1.2.0-macos-x86_64.zip

X509 Help:

    http://github.com/ramdhakne/blogs/blob/master/external-connectivity/x509-help.txt

Minikube YAML:

    http://raw.githubusercontent.com/ramdhakne/blogs/master/minikube/couchbase-persistent-cluster-tls-k8s-minikube.yaml

App pod YAML:

    http://raw.githubusercontent.com/ramdhakne/blogs/master/external-connectivity/assets/app_pod.yaml

Python SDK example:

    http://raw.githubusercontent.com/ramdhakne/blogs/master/external-connectivity/python/python_sdk_example.py

Inspired by:

    http://blog.couchbase.com/tutorial-running-couchbase-autonomous-operator-on-minikube-with-sample-app/

[Note that this blog post covers running `minikube` on MacOS with `VirtualBox` while I cover linux and `Docker`.]
