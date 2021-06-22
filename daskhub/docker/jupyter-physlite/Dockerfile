# Documentation: https://zero-to-jupyterhub.readthedocs.io/en/latest/jupyterhub/customizing/user-environment.html
FROM pangeo/base-notebook:2020.11.06
USER root
RUN apt-get update --yes && apt-get install --yes git openssh-client
USER jovyan
ENV PATH=/srv/conda/envs/notebook/bin:$PATH
RUN pip install --no-cache-dir rucio-clients
RUN cp /srv/conda/envs/notebook/etc/rucio.cfg.atlas.client.template /srv/conda/envs/notebook/etc/rucio.cfg
RUN pip install --no-cache-dir numpy h5py numba uproot awkward pyarrow coffea aiohttp
RUN pip install --no-cache-dir google.cloud.bigquery_storage google-cloud-bigquery
RUN pip install --no-cache-dir git+https://gitlab.cern.ch/nihartma/physlite-experiments@723fa0a3f615514843b64e2e6ebcf85e6b44cfc4
COPY 0001-Bugfix-check-for-file-like-objects-in-from_parquet.patch /srv/conda/envs/notebook/lib/python3.8/site-packages/awkward/
WORKDIR /srv/conda/envs/notebook/lib/python3.8/site-packages/awkward
RUN patch -p3 < 0001-Bugfix-check-for-file-like-objects-in-from_parquet.patch
WORKDIR /home/jovyan