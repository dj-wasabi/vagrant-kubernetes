# Cluster Architecture, Installation & Configuration

## Upgrade Cluster

1. Update the whole cluster
2. Create backup of ETCD and store it as `/tmp/snapshot.db`.
3. Remove all pods from worker `node-2`. Create a deployment named `nginx` with image `nginx:1.19.1` and run 6 replicas. Verify that these are running. Make sure that no new pods can be scehduled onto `node-2` and remove all running pods. Verify that it is succeeded and then allow scheduling again.

<details>
<summary>1. Update the whole cluster.</summary>
<br>
To long with lots of details. Please make sure you have following all the steps found on the official Kubernetes documentation the following page: https://kubernetes.io/docs/tasks/administer-cluster/kubeadm/kubeadm-upgrade/
</details>

<details>
<summary>2. Create backup of ETCD.</summary>
<br>

Execute:

    $ sudo ETCDCTL_API=3 etcdctl --cacert /etc/kubernetes/pki/etcd/ca.crt --cert /etc/kubernetes/pki/etcd/healthcheck-client.crt --key /etc/kubernetes/pki/etcd/healthcheck-client.key snapshot save /tmp/snapshot.db

</details>

<details>
<summary>3. Remove all pods from worker `node-2`.</summary>
<br>

Execute:

    $ kubectl create deployment nginx --image=nginx:1.19.1 --replicas=6
    $ kubectl get pods -o wide
    $ kubectl drain worker-2 --ignore-daemonsets
    $ kubectl get pods -o wide
    $ kubectl uncordon worker-2

</details>