# Configuration

## ConfigMaps

1. Create a configMap named `matters` and add the following keys:
* WHO=`<Your Name>`
* WHAT=Awesome
2. Create a Pod named `who-is-awesome` with the `nginx` image and make sure that both `WHO` and `WHAT` are set as environment variables.

**Solutions**

<details>
<summary>1. Create a configMap.</summary>
<br>

Execute:

    $ kubectl create configmap matters --from-literal=WHO=Werner --from-literal=WHAT=pizza

</details>

<details>
<summary>2. Create a Pod.</summary>
<br>


    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        run: who-is-awesome
      name: who-is-awesome
    spec:
      containers:
      - image: nginx
        name: who-is-awesome
        env:
        - name: WHO
          valueFrom:
            configMapKeyRef:
              name: matters
              key: WHO
        - name: WHAT
          valueFrom:
            configMapKeyRef:
              name: matters
              key: WHAT

Execute:

    $ kubectl create -f who-is-awesome.yaml
    $ kubectl exec -it who-is-awesome -- env | egrep 'WHO|WHAT'
    WHAT=pizza
    WHO=Werner

</details>

## Secrets

1. Create a secret named `db-info` with the following keys:

* DB_HOST=mysql.host.com
* DB_USERNAME=pizza
* DB_PASSWORD=ILoveIt

2. Create a Pod named `frontend` with the `nginx` image and set with one entry all secrets as environment variables in the container. 

**Solutions**

<details>
<summary>1. Create a Secret.</summary>
<br>

    $ kubectl create secret generic db-info --from-literal=DB_HOST=mysql.host.com --from-literal=DB_USERNAME=pizza --from-literal=DB_PASSWORD=ILoveIt

</details>

<details>
<summary>2. Create a Pod.</summary>
<br>

frontend-secret.yaml:

    apiVersion: v1
    kind: Pod
    metadata:
      labels:
        run: frontend
      name: frontend
    spec:
      containers:
      - image: nginx
        name: frontend
        envFrom:
        - secretRef:
            name: db-info

Execute:

    $ kubectl create -f frontend-secret.yaml
    $ kubectl exec -it frontend -- env | grep ^DB
    DB_PASSWORD=ILoveIt
    DB_USERNAME=pizza
    DB_HOST=mysql.host.com

</details>

## Requests and limits

1. Create Nginx POD with CPU and MEM limits. CPU should have a limit of 200m and Memory of 128Mi.
2. Create Nginx POD with CPU and MEM request. CPU should have a request of 2 and Memory of 3072Mi. See what happens.

**Solutions**

<details>
<summary>1. Create Nginx POD with CPU and MEM limits.</summary>
<br>

Execute:

    $ kubectl run nginx --image=nginx --limits='cpu=200m,memory=128Mi'

</details>

<details>
<summary>2. Create Nginx POD with CPU and MEM request.</summary>
<br>

Execute:

    $ kubectl run nginx --image=nginx --requests='cpu=2,memory=3072Mi'
    $ kubectl describe pod nginx-<TAB> # See why it is failing.

</details>
