# Documentation: https://zero-to-jupyterhub.readthedocs.io/en/latest/jupyterhub/customizing/user-environment.html
FROM pangeo/base-notebook:2023.07.05
USER root
RUN apt-get update --yes && apt-get install --yes git openssh-client
USER jovyan
ENV PATH=/srv/conda/envs/notebook/bin:$PATH
RUN pip install --no-cache-dir rucio-clients
RUN cp /srv/conda/envs/notebook/etc/rucio.cfg.atlas.client.template /srv/conda/envs/notebook/etc/rucio.cfg
RUN pip install --no-cache-dir numpy h5py numba pyarrow aiohttp
RUN pip install --no-cache-dir google.cloud.bigquery_storage google-cloud-bigquery
RUN pip install --no-cache-dir 'uproot>=5' 'awkward>=2' 'coffea==v2023.7.0.rc0' vector particle 'hist[plot]'
