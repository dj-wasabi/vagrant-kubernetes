# Storage / ConfigMaps / Secrets

## Storage

1. Create a Persistent Volume and create a hostPath volume of 5GB on `/tmp/pv-html`. Give it `ReadWriteOnce` mode and name it `pv-html`.;
2. Create a Persistent Volume Claim with name `claim-html` and make sure that you requests at least 4GB and `ReadWriteOnce`;
3. Create a Pod with the `nginx` image named `frontend` and make sure the `/usr/share/nginx/html` stores it data on the earlier created Persisted Volume;

**Solutions**

<details>
<summary>1. Create a Persistent Volume.</summary>
<br>
pv.yaml:

    apiVersion: v1
    kind: PersistentVolume
    metadata:
      name: pv-html
    spec:
      capacity:
        storage: 5Gi
      volumeMode: Filesystem
      accessModes:
        - ReadWriteOnce
      hostPath:
        path: /tmp/pv-html

Execute:

    kubectl create -f pv.yaml

</details>

<details>
<summary>2. Create a Persistent Volume Claim.</summary>
<br>
pvc.yaml

    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: claim-html
    spec:
      accessModes:
        - ReadWriteOnce
      volumeMode: Filesystem
      resources:
        requests:
          storage: 4Gi

Execute:

    kubectl create -f pvc.yaml

</details>

<details>
<summary>3. Create a Pod.</summary>
<br>

pod.yml

    apiVersion: v1
    kind: Pod
    metadata:
      name: mypod
    spec:
      containers:
        - name: frontend
          image: nginx
          volumeMounts:
          - mountPath: "/var/www/html"
            name: html-claim
      volumes:
        - name: html-claim
          persistentVolumeClaim:
            claimName: claim-html

Execute:

    kubectl create -f pod.yaml

</details>

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

    kubectl create secret generic db-info --from-literal=DB_HOST=mysql.host.com --from-literal=DB_USERNAME=pizza --from-literal=DB_PASSWORD=ILoveIt

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
