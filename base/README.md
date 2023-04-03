# Base directory
This directory serves as the base configuration for our deployments and a reference point to overlay. The main file here is **kustomization.yaml**, that defines what all resources should be used. 

* Here we intend to use *deployment.yaml* & *service.yaml* for our application deployment requirement that creates k8s deployment and service objects for us.
* Here we also define some common environment variables via *base.env* file that we would like to use with our deployments.

## ConfigMap generation
Kustomize provides 2 ways of defining ConfigMap, either by passing configmap as a resource or declaring from a ConfigMapGenerator.

> ```
> # declare ConfigMap as a resource
> resources:
> - configmap.yaml
>
> # declare ConfigMap from a ConfigMapGenerator
> configMapGenerator:
> - name: a-configmap
>   files:
>     # configfile is used as key
>     - configs/configfile
>     # configkey is used as key
>     - configkey=configs/another_configfile
> ```
* In our case, we are generating a ConfigMap via configMapGenerator and using environment variables from *base.env*.

```
configMapGenerator:
- name: sample-config-map
  envs:
    - base.env
```    