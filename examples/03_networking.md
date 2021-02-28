# Services & Networking

1. Create a Network Policy. Create 3 different deployments named `deployment1`, `deployment2` and `deployment3` with a `busybox` image and create a Network Policy that only allow traffic from `deployment1` to `deployment2`;
2. Create a pod named `web-01` with the `nginx` image and a service named `web` on a nodePort;

**Solutions**

<details>
<summary>1. Create a Network Policy</summary>
<br>

deployment1.yml

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: deployment1
    spec:
      selector:
        matchLabels:
          app: deployment1
      template:
        metadata:
          labels:
            app: deployment1
        spec:
          containers:
            - name: deployment1
              image: busybox
              command: ['sh', '-c', 'sleep 1000']

deployment2.yml

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: deployment2
    spec:
      selector:
        matchLabels:
          app: deployment2
      template:
        metadata:
          labels:
            app: deployment2
        spec:
          containers:
            - name: deployment2
              image: busybox
              command: ['sh', '-c', 'sleep 1000']

deployment3.yml

    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: deployment3
    spec:
      selector:
        matchLabels:
          app: deployment3
      template:
        metadata:
          labels:
            app: deployment3
        spec:
          containers:
            - name: deployment3
              image: busybox
              command: ['sh', '-c', 'sleep 1000']

policy.yaml

    apiVersion: networking.k8s.io/v1
    kind: NetworkPolicy
    metadata:
      name: deployment-npolicy
    spec:
      podSelector:
        matchLabels:
          app: deployment2
      ingress:
      - from:
        - podSelector:
            matchLabels:
              app: deployment1

Execute commands

    $ kubectl create -f deployment1.yml
    $ kubectl create -f deployment2.yml
    $ kubectl create -f deployment3.yml
    $ kubectl get pods -o wide
    $ kubectl exec -it deployment3-* -- ping <IP_OF_POD_2>

With ping data:

    $ kubectl exec -it deployment3-7ff5895d5b-f2sxs -- ping 192.168.226.67
    PING 192.168.226.67 (192.168.226.67): 56 data bytes
    64 bytes from 192.168.226.67: seq=0 ttl=63 time=0.139 ms
    64 bytes from 192.168.226.67: seq=1 ttl=63 time=0.082 ms

Configure Policy

    $ kubectl create -f policy.yml
    $ kubectl exec -it deployment3-* -- ping <IP_OF_POD_2>

No ping data

    $ kubectl exec -it deployment3-7ff5895d5b-f2sxs -- ping 192.168.133.195
    PING 192.168.133.195 (192.168.133.195): 56 data bytes
    ^C
    --- 192.168.133.195 ping statistics ---
    3 packets transmitted, 0 packets received, 100% packet loss
    command terminated with exit code 1

</details>


<details>
<summary>2. Create a Pod and Service.</summary>
<br>

    kubectl run web-01 --image=nginx --port 80
    kubectl expose pod web-01 --name web --type=NodePort
    vagrant@control:~$ curl http://10.10.1.11:32289
    <!DOCTYPE html>
    <html>
    <head>
    <title>Welcome to nginx!</title>
    ...

</details>
