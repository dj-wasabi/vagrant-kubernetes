# Troubleshooting

1. Failure while deploying a deployment. On the `control` node on location `/opt/resources/01_failing_deployment.yaml` a deployment file exist. But for some reason, executing `kubectl create -f /opt/resources/01_failing_deployment.yaml` fails. Please fix and deploy the deployment.

<details>
<summary>1. Failure while deploying a deployment</summary>
<br>

The labels mentioned on line 15 is incorrect. It mentions frontend=nginx, but should be app=frontend. 

</details>