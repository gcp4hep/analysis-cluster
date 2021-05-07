# DaskHub installation and usage

## Installation 

Official daskhub installation instructions are here: https://docs.dask.org/en/latest/setup/kubernetes-helm.html

Helm and other pre-requisites have to be followed from the official documentation. The file `values.yaml` contains the configuration 
of our current installation.  
```
helm upgrade --wait --install --render-subchart-notes dhub dask/daskhub --values=secrets.yaml
```

## Explanations for relevant sections in our `values.yaml` configuration 

### Tokens
The tokens are generated using:  
```
openssl rand -hex 32
```
### JupyterHub
Users and administrators for JupyterHub are added to the configuration file, until there is a
more advanced option (e.g. OAuth).

### Dask gateway
#### Traefik service
Note the service for dask-gateway is marked as LoadBalancer. This is to be able to connect 
to Dask Gateway directly from outside the K8s cluster.

#### optionHandler
By default Dask Gateway does not allow users to overwrite worker size (CPU, memory) 
or image configuration. This has to be enabled explicitly.

## Usage

### Through JupyterHub

You need to request a JupyterHub user account and link to the cluster administrator.

Each user runs on an independent pod. For example the current image is `pangeo/base-notebook:2020.11.06`, but this
will evolve over time. At this stage we have not evaluated possibilities of providing different `conda` environments or
other customizations. 

Each user is assigned a 10GB disk for his personal files.

![Hello](https://github.com/gcp4hep/analysis-cluster/blob/main/daskhub/images/dg_basic.png)

