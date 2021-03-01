# Troubleshooting

1. Failure while deploying a deployment. On the `control` node on location `/opt/resources/01_deployment.yaml` a deployment file exist. But for some reason, executing `kubectl create -f /opt/resources/01_deployment.yaml` fails. Please fix and deploy the deployment.
2. Deploy the deployment found on location `/opt/resources/02_deployment.yaml`. Fix the issue.

**Solutions**

<details>
<summary>1. Failure while deploying a deployment</summary>
<br>

The labels mentioned on line 15 is incorrect. It mentions frontend=nginx, but should be app=frontend. 

</details>

<details>
<summary>1. Failure while deploying a deployment</summary>
<br>

The image is wrong, it is `ngninx:1.19.2` when it should be `nginx:1.19.2`. 

Execute

    $ kubectl create -f /opt/resources/02_deployment.yaml

The labels mentioned on line 15 is incorrect. It mentions frontend=nginx, but should be app=frontend. 

</details>