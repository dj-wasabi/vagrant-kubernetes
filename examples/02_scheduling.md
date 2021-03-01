# Workloads & Scheduling

1. Use a nodeSelecter to run a pod named `frontend` with image `nginx` on a node that has label `hosttype=frontend`. Add that lable to the node `worker-02`;
2. Create a default quota for memory usage for 128Mi on namespace `resource`. Create a pod named `web-frontend` with image `nginx` and set the request to 64Mi and limit to 256Mi. After that, apply change to the pod configuration to deploy the pod;
3. Create a deployment named `web-nginx` with image `nginx:1.19.2` and run it with 3 replicas. Scale the deployment to 7 replicas. See what happens. Once done, update the image to `nginx:1.19.7` and see what happens. Now do a rollback of a deployment and verify which Nginx image is used.

**Solutions**

<details>
<summary>1. Use a nodeSelector.</summary>
<br>
nodeSelector.yaml:

    apiVersion: v1
    kind: Pod
    metadata:
      name: frontend
    spec:
      containers:
        - name: frontend
          image: nginx
      nodeSelector:
        hosttype: frontend

Execute:

    $ kubectl label nodes worker-2 hosttype=frontend
    $ kubectl create -f nodeSelector.yaml

</details>

<details>
<summary>2. Create a default quota.</summary>
<br>
limit.yaml:

    apiVersion: v1
    kind: LimitRange
    metadata:
      name: tenant-max-mem
    spec:
      limits:
      - max:
          memory: 128Mi
        type: Container

web-frontend.yaml:

    apiVersion: v1
    kind: Pod
    metadata:
      name: web-frontend
    spec:
      containers:
        - name: web-frontend
          image: nginx
          resources:
            requests:
              memory: "64Mi"
            limits:
              memory: "256Mi"

Execute:

    $ kubectl create ns resource
    $ kubectl create -n resource -f limit.yaml
    $ kubectl create -f -n resource web-frontend.yaml
    Error from server (Forbidden): error when creating "web.yaml": pods "web-frontend" is forbidden: maximum memory usage per Container is 128Mi, but limit is 256Mi

</details>

<details>
<summary>3. Create a deployment web-nginx.</summary>
<br>

Execute:

    $ kubectl create deployment web-nginx --image=nginx --replicas=3
    $ kubectl scale deployment web-nginx --replicas=7
    $ kubectl set image deployment/web-nginx nginx=nginx:1.19.7
    $ kubectl rollout undo deployment web-nginx
    $ kubectl describe pod web-nginx-*-* | grep 'Image:'

</details>