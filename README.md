# Kubernetes

## Service & Pod
![a3862ac0c931ec5f8aded496c0fd83e5.png](./_resources/a3862ac0c931ec5f8aded496c0fd83e5.png)
![1ec4fb73710c2cfb5be2fa73c00d2324.png](./_resources/1ec4fb73710c2cfb5be2fa73c00d2324.png)

## Deployments
![34e46cdab3c3d88bbf9b4bfd4ebe4a11.png](./_resources/34e46cdab3c3d88bbf9b4bfd4ebe4a11.png)

## Rollout
![fe8b3c9df926ae310e0574ef31a61cf2.png](./_resources/fe8b3c9df926ae310e0574ef31a61cf2.png)

## Network
![be8aa0e3140fd25e85b88187a661df56.png](./_resources/be8aa0e3140fd25e85b88187a661df56.png)

## Namespace
![1da502dbb84754708b6636492bbccf6b.png](./_resources/1da502dbb84754708b6636492bbccf6b.png)

`nslookup domain_name`

## Logging

![337570299fc7d536d854a7c1d3613236.png](./_resources/337570299fc7d536d854a7c1d3613236.png)
![a54cd00a3238eb9b79e57076afa01424.png](./_resources/a54cd00a3238eb9b79e57076afa01424.png)

## Monitoring

- Prometheus
- Grafana

## Alert

- Prometheus Alerts
- AlertManager
- Integrate with Slack
- Integrate with PagerDuty

## What if the mast node done?
no impact on running workloads, but pods cannot be scheduled before master node recover, kubectl cannot be used as well

## Requests and Limits
- Requests: hint for master node when schedule a pod about how much resources the pod can run comfortablely , no impact on runtime resource utilization, just hint
- Limits: runtime impact on resource utitlization
  - cpu: how many cpu resource can be used
  - memory: upper limits of the memory usage, container will be killed once reach this limit

## Metrics profiling
- minikube addons: 
	- metrics-server
	  - `kubectl top pod`
      - `kubectl top node`
      - `kubectl describe node mimikube`
    - heapster (deprecated)
	- dashboard
- set resonable Requests as per the metrics

## Horizontal Pod AutoScale
![7e164abe86c0cba0deaa52987ba9bb66.png](./_resources/7e164abe86c0cba0deaa52987ba9bb66.png)

`kubectl get hpa`
`kubectl get hpa api-gateway -o yaml`

## Readiness and Liveness probe
![076e3f17fbc81c666983590ded7e9500.png](./_resources/076e3f17fbc81c666983590ded7e9500.png)

## Quality of Service and Eviction
![2ccc983612cba89d8dc19fe6162dc9ac.png](./_resources/2ccc983612cba89d8dc19fe6162dc9ac.png)

- Pod priority

## ConfigMap and Secret

- configmap change will not hot reload for the relevant deployments which is using it, need to restart them
  - one option is using [reloader](https://github.com/stakater/Reloader/)
  - for spring, an option is spring cloud kubenetes or spring cloud config

use config map as
- env
  - single env value ref to config map
  - env section from config map
- mounted file
versioning the config to update the references to make it work

## Ingress controller

![273beb4c501088a2b5183082330c84ad.png](./_resources/273beb4c501088a2b5183082330c84ad.png)
- Authentication

## Other workloads

- Batch Jobs
- Cron Jobs
- DaemonSets
  fluentd, logstash
- StatefulSets
  ![106fb877913371086c099f08cca4027b.png](./_resources/106fb877913371086c099f08cca4027b.png)
  key point of "stateful" means the name of the pod is stateful, usage example: elasticsearch cluster, kafka cluster, database replication

## CI/CD

For jenkins, the key files are:
- DockerFiler
- JenkisFile
- deploy.yaml # which includes all the related components to this micro-service need to be deployed 

## Helm

Cmd for ad-hoc changes(bad smell, introduce snowflake cluster):
- helm install
- helm uninstall
- helm list
- helm show values
- helm upgrade

Cmd for get fixed version of Charts:
- helm pull
- helm install xxx [localpath]
- helm upgrade xxx [localpath] --values=customvalues.yaml

Cmd to use dynamic yaml(replace var in the yaml with values.yaml):
- help template

Create self custom Charts:
- helm create
- helm template
- template, values, go lang, {{ ... }}
- SubTemplate







# Common cmd

`kubectl get all`
(Section 5): list all objects that you’ve created. Pods at first, later, ReplicaSets, Deployments and Services
`kubectl apply –f <yaml file>`
(Section 5): either creates or updates resources depending on the contents of the yaml file
`kubectl apply –f .`
(Section 7): apply all yaml files found in the current directory
`kubectl describe pod <name of pod>`
(Section 5): gives full information about the specified pod
`kubectl exec –it <pod name> <command>`
(Section 5): execute the specified command in the pod’s container. Doesn’t work well in Cygwin.
`kubectl get (pod | po | service | svc | rs | replicaset | deployment | deploy)`
(Section 6): get all pods or services. Later in the course, replicasets and deployments.
`kubectl get po --show-labels`
(Section 6): get all pods and their labels
`kubectl get po --show-labels -l {name}={value}`
(Section 6): get all pods matching the specified name:value pair
`kubectl delete po <pod name>`
(Section 8): delete the named pod. Can also delete svc, rs, deploy
`kubectl delete po --all`
(Section 8): delete all pods (also svc, rs, deploy)
`kubectl rollout status deploy <name of deployment>`
(Section 9): get the status of the named deployment
`kubectl rollout history deploy <name of deployment>`
(Section 9): get the previous versions of the deployment
`kubectl rollout undo deploy <name of deployment>`
(Section 9): go back one version in the deployment. Also optionally -- to-revision=<revision number>
We recommend this is used only in stressful emergency situations! Your YAML will now be out of date with the live deployment!