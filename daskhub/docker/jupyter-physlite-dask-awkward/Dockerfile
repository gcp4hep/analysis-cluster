# Documentation: https://zero-to-jupyterhub.readthedocs.io/en/latest/jupyterhub/customizing/user-environment.html
FROM pangeo/base-notebook:2023.07.05
USER root
RUN apt-get update --yes && apt-get install --yes git openssh-client graphviz htop
USER jovyan
ENV PATH=/srv/conda/envs/notebook/bin:$PATH
RUN pip install --no-cache-dir rucio-clients
RUN cp /srv/conda/envs/notebook/etc/rucio.cfg.atlas.client.template /srv/conda/envs/notebook/etc/rucio.cfg
RUN pip install --no-cache-dir numpy h5py numba pyarrow aiohttp zstandard graphviz
RUN pip install --no-cache-dir google.cloud.bigquery_storage google-cloud-bigquery
RUN pip install --no-cache-dir 'uproot>=5' 'awkward>=2.3.3' vector particle 'hist[plot]' dask-histogram
RUN pip install --no-cache-dir 'git+https://github.com/nikoladze/coffea@44141e9a0a278a879d2c457c93cdd8b7f95ba068'
