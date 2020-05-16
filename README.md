# cicd-pipeline-train-schedule-autoscaling

This is a simple train schedule app written using nodejs. It is intended to be used as a sample application for a series of hands-on learning activities.

## Running the app

You need a Java JDK 7 or later to run the build. You can run the build like this:

    ./gradlew build

You can run the app with:

    ./gradlew npm_start

Once it is running, you can access it in a browser at http://localhost:8080


Learning Objectives
check_circle
Install the Kubernetes metrics API in the cluster.
To accomplish this, do the following:
Clone the Kubernetes metrics repo.
git clone https://github.com/kubernetes-incubator/metrics-server.git
Apply the standard configurations to install the Metrics API.
cd metrics-server/
git checkout ed0663b3b4ddbfab5afea166dfd68c677930d22e
kubectl create -f deploy/1.8+/
Wait a few seconds for the metrics server pods to start. You can see their status with kubectl get pods -n kube-system.
check_circle
Configure a Horizontal Pod Autoscaler to autoscale the train schedule app.
Check the example-solution branch of the source code repo for an example of the code changes needed in the train-schedule-kube.yml file: https://github.com/linuxacademy/cicd-pipeline-train-schedule-autoscaling/blob/example-solution/train-schedule-kube.yml.
To complete this task, you will need to do the following:
Create a fork of the source code at https://github.com/linuxacademy/cicd-pipeline-train-schedule-autoscaling.
Add a CPU resource request in train-schedule-kube.yml for the pods created by the train-schedule deployment. Check the example solution if you need to need to know where to add this.
resources:
  requests:
    cpu: 200m
Define a HorizontalPodAutoscaler in train-schedule-kube.yml to autoscale in response to CPU load.
---

apiVersion: autoscaling/v2beta1
kind: HorizontalPodAutoscaler
metadata:
  name: train-schedule
  namespace: default
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: train-schedule-deployment
  minReplicas: 1
  maxReplicas: 4
  metrics:
  - type: Resource
    resource:
      name: cpu
      targetAverageUtilization: 50
Generate some load on the app to see the autoscaler in action!
