---
title: "1.4 Uptime app"
weight: 14
sectionnumber: 1.4
---

In this lab you're going to deploy an application on your freshly-installed OpenShift cluster.

The application's availability will be monitored and recorded by an external monitoring system.
Your main goal for the rest of this training is to keep this application up and running, no matter what you do.

{{% alert title="Note" color="primary" %}}
At any time you can view the availability of your cluster console and the uptime app on [this](https://grafana-ops.training.acend.ch/d/7KmbRHXGz/ocp4monitoring?orgId=1&refresh=5s&var-userdropdown=+username+) dashboard. Credentials to login will be provided by your trainers.
{{% /alert %}}


## Uptime app

The application you're going to install is a simple Python app. It's going to be scaled to 3 replicas in order to achieve high availability.


## Task {{% param sectionnumber %}}.1: Deploy the uptime app

First you need to create a new project named `uptime-app-prod` for the application.

{{% details title="Hints" mode-switcher="normalexpertmode" %}}

```bash
oc new-project uptime-app-prod
```

{{% /details %}}

Next, you're going to create the following resources:

* Deployment
* Service
* Route

To deploy these resources, you can simply execute the following command:

```bash
oc apply -f https://raw.githubusercontent.com/acend/openshift-4-ops-training/main/content/en/docs/01/resources/uptime-app.yaml -n uptime-app-prod
```

What's left is to adapt the hostname:

```bash
oc patch route uptime-app --patch '{"spec": {"host": "uptime-app-uptime-app-prod.apps.+username+-ops-training.openshift.ch"}}' -n uptime-app-prod
```

The deployed application should now be available at <https://uptime-app-uptime-app-prod.apps.+username+-ops-training.openshift.ch>.

With 3 replicas and the [Pod anti-affinity](https://docs.openshift.com/container-platform/latest/nodes/scheduling/nodes-scheduler-pod-affinity.html#nodes-scheduler-pod-affinity-example-antiaffinity_nodes-scheduler-pod-affinity), which will make sure all 3 Pods are never deployed on the same underlying node, we have a good setup for the training to continue.
