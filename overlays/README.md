# Overlay directory
This directory serves as the container for the configuration that we would like to implement over the base configuration, plus if any changes required in the base configuration before the deployment happens.

We may have separate environments, ie. *dev | uat | stage | prod*, etc, where we would want to use the same base confiugration, however with some changes depending upon the environment.

We can notice, we are defining 2 different environments here and each of them uses **kustomization.yaml** to define what **base** should be referenced for the base configuration + any changes required on top of that.

Let's take example of Environment 1:

* We are defining a **namespace** where the resources should be deployed for this environment.
* Then we are referencing resources from the **base** directory. This means it will use *deployment.yaml* & *service.yaml* from base directory.

```
resources:
  - ../../base
```
And then we are also adding *autoscaling.yaml* on top of base. 

* Any **labels** we pass here will be added to the every resource deployed for this environment.

* **Images** section helps us to override the container image & version defined in these directories. e.g below is deployment object (from base) where we have defined nginx:latest image.

```
apiVersion: apps/v1
kind: Deployment
....
    spec:
      containers:
        - name: app
          imagePullPolicy: Always
          image: nginx:latest
          resources:
...          
```
However, this section will override the image name and version, after matching it against the original name and version.
```
images:
  - name: nginx:latest
    newName: 123456789100.dkr.ecr.ap-northeast-1.amazonaws.com/namespace1/java
    newTag: v1.0.0
```

*  **patchesStrategicMerge** field helps to apply a patch on an existing kubernetes object. The names inside the patches must match Resource names that are already loaded. Again taking an example of **deployment-1** deployment in base directory:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
spec:
...
    spec:
      containers:
        - name: app
          ...
          resources:
            requests:
              cpu: "50m"
              memory: "100Mi"
...          
```

I can patch a **resources.limits** section into this deployment which was not there in the original manifest. I just need to create a new file: patch-deployment.yaml with below content and put it under this section.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: deployment-1
spec:
...
    spec:
      containers:
        - name: app
          resources:
            requests:
              cpu: "50m"
              memory: "100Mi"
            limits:
              cpu: "50m"
              memory: "150Mi"
...          
```


