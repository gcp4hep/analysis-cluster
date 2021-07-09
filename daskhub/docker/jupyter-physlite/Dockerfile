# Documentation: https://zero-to-jupyterhub.readthedocs.io/en/latest/jupyterhub/customizing/user-environment.html
FROM pangeo/base-notebook:2021.07.06
USER root
RUN apt-get update --yes && apt-get install --yes git openssh-client
USER jovyan
ENV PATH=/srv/conda/envs/notebook/bin:$PATH
RUN pip install --no-cache-dir rucio-clients
RUN cp /srv/conda/envs/notebook/etc/rucio.cfg.atlas.client.template /srv/conda/envs/notebook/etc/rucio.cfg
RUN pip install --no-cache-dir numpy h5py numba pyarrow aiohttp
RUN pip install --no-cache-dir google.cloud.bigquery_storage google-cloud-bigquery
RUN pip install --no-cache-dir uproot 'awkward>=1.4' coffea vector particle 'hist[plot]'
RUN pip install --no-cache-dir git+https://gitlab.cern.ch/nihartma/physlite-experiments@4b872add80af1a95b532bfb2d456784180eefee7
