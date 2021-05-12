# DaskHub installation and usage

## Installation 

This documentation will refer to our installation process of Dask Gateway + JupyterHub on a Kubernetes backend, in particular
using the `daskhub` helm chart. The official `daskhub` installation instructions are here: https://docs.dask.org/en/latest/setup/kubernetes-helm.html

Helm and other pre-requisites have to be followed from the official documentation. The file `values.yaml` contains our 
desired configuration .
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

Below you will find information on how to use the Dask Gateway installation and basic snippets for getting you started. 
Further Dask usage needs to be consulted in the official Dask pages. 

### Through JupyterHub

You need to request a JupyterHub user account and link to the cluster administrator.

Each user runs on an independent pod. For example the current image is `pangeo/base-notebook:2020.11.06`, but this
will evolve over time. At this stage we have not evaluated possibilities of providing different images and/or `conda` environments. 

Each user is assigned a 10GB disk for his personal files.

#### Basic creation and shutdown of a Dask cluster

The following figure illustrates a basic example. 
1. You connect to the gateway (no configuration needed!), create a cluster and get the client. You can scale the cluster
to the required size.
```
from dask_gateway import GatewayCluster
cluster = GatewayCluster(worker_cores=1, worker_memory=2, image="xxx/yyy:zzz")
client = cluster.get_client()
```
It's also possible to create a cluster by clicking on `CLUSTERS ... +NEW` on the left panels. However currently 
this will generate a LOCAL cluster, i.e. living in your JupyterHub pod.

2. You will get a `Dashboard` URL. Copy paste it (including the IP!) to the top-left Dask widget, if you want to enable
   fancy displays.

2. Run your computation, in this example interact with a dask array.

2. Shutdown the cluster. If you don't shut down the cluster, it will stay around occupying resources until you disconnect.
```
cluster.shutdown()
```

![Hello](https://github.com/gcp4hep/analysis-cluster/blob/main/daskhub/images/dg_basic.png)

#### Dask cluster management

> :warning: Please use the system carefully and don't leave idle clusters around

Also note that (not shown in the previous image) you can interact with the existing clusters. You don't need a new cluster each time.
```
from dask_gateway import Gateway
gateway = Gateway()
clusters = gateway.list_clusters()
cluster = gateway.connect(clusters[0].name)
# RUN YOUR COMPUTATION
cluster.shutdown()
```
In order to delete all your old clusters, you can run:
```
from dask_gateway import Gateway
gateway = Gateway()
clusters = gateway.list_clusters()
for cluster in clusters:
    print ("Stopping cluster {0}".format(cluster.name))
    gateway.stop_cluster(cluster.name)
```

### Interact directly with Dask Gateway

If you want to interact with DaskGateway directly through python:
1. Generate a JupyterHub token from `http://<JupyterHub address>/hub/token` and save it.
1. Then from your console connect to Dask. The IP is the Dask Gateway IP, NOT the IP for JupyterHub.
```
export JUPYTERHUB_API_TOKEN=<YOUR TOKEN>
[user@machine gke-dask]# python3
Python 3.6.8 (default, Nov 16 2020, 16:55:22)
[GCC 4.8.5 20150623 (Red Hat 4.8.5-44)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from dask_gateway import Gateway
>>> gateway = Gateway("http://<DASK GATEWAY IP>/services/dask-gateway", auth='jupyterhub')
>>> cluster = gateway.new_cluster(image='fbarreir/daskgateway:latest')
>>> client = cluster.get_client()
>>> # RUN YOUR COMPUTATION
```

### User and worker environment

User and worker environment need to match. That means if you are using a particular package, compatible versions of it need to be installed on your user environment (e.g. your jupyter session or your python client) and the Dask workers. In the case of a LOCAL cluster client and workers run on the same environment and you don't need to pay attention on matching environments. 

We are currently exploring the best way to have a stable setup that covers the requirements of the users (but we need to know what the users want).

| Jupyter image | Conda env | Worker image | Description |
| ------------- | ------------- | ----- | ----- |
| [pangeo/base-notebook:2020.11.06](https://github.com/pangeo-data/pangeo-docker-images) | notebook | [daskgateway/dask-gateway:0.9.0](https://github.com/dask/dask-gateway/tree/main/dask-gateway) | Basic Dask installation |
