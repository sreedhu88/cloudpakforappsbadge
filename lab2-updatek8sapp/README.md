## Prerequisites

- You will need an IBM Account for [IBM Cloud](https://cloud.ibm.com/)
- Downloaded and installed the IBM Cloud CLI at [IBM Cloud CLI Installation](https://cloud.ibm.com/docs/cli?topic=cloud-cli-getting-started#step1-install-idt). Follow the instructions for your computer’s operating system.
- Downloaded and installed the OpenShift CLI at [OpenShift CLI Installation](https://OpenShift.io/docs/tasks/tools/install-kubectl/). Follow the instructions for your computer’s operating system.
- Downloaded and installed the Kubernetes CLI at [Kubernetes CLI Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/). Follow the instructions for your computer’s operating system.
- Installing `watch` command - For Mac run `brew install watch`. No install for Windows

## Scaling out your app

Now that we have successfully deployed an application on Kubernetes, the next step is to scale up the application to properly handle more traffic and have some redundancy to prevent any downtime.

Let’s first check our current deployment running on OpenShift. (If there is no deployment refer to *Lab 3:Deploy Kubernetes Application*

Check Deployment

```shell
oc get deployment greetings-deployment
```

![current deployment](images/current-deployment.png)

We can see that only 1 replica of the application is currently desired and is all that is running. Now let’s make it to where we have 3 desired replicas and see how Kubernetes automatically deploys more replicas to meet the desired state by using the `oc scale` command.

More Replicas

```shell
oc scale deployment greetings-deployment --replicas 3
```

The command above will scale the sample application to 3 replicas and you can verify the replicas by running one of the following:

Verify Replicas

```shell
oc get deployment greetings-deployment
```

![scaled deploy](images/scaled-deploy.png)

Verify Pods

```shell
oc get pods
```

![scaled pods](images/scaled-pods.png)

Replication is easy with Kubernetes and can be changed with one simple command at any time. Next lets look at updating the replicas with new verions of code.

## Update your app

Updating applications with little to no downtime is a huge advantage for using Kubernetes. In this next section we are going to walkthrough how easy it is.

The first step is changing the docker image to the new version (v2) that we have pushed to Docker Hub and begin the rolling update:

Updating the Deployment

```shell
oc set image deployment/greetings-deployment greeting=ibmcase/greeting:v2
```

Next, we want to monitor the rollout and watch as containers are created and deployed.

Monitor the Rollout

```shell
oc rollout status deployment/greetings-deployment
```

Finally, once the rollout is complete we want to recheck our application and see the new changes that were pushed.

First we need to get the `route` in order to curl the application

Get Route

```shell
oc get route
```

Next copy the `PATH` it will look similar to `greeting-service-deploy-sample.gse-cloud-native-fa9ee67c9ab6a7791435450358e564cc-0001.us-east.containers.appdomain.cloud`. Finally we want to run a `curl` against the copied path with `/greeting` appended on the end to see the response.

Curl

```shell
curl PATH/greeting
```

You should get a response similar to: `Welcome to Version 2 of the Cloud Native Bootcamp Application !!!`

### Conclusion

You have successfully completed this lab! Let’s take a look at what you learned and did today:

- Scaled a deployed application to 3 replicas.
- Accessed the application through a CURL command.
- Updated a deployed application to a newer image version.


