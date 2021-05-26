# Documentation: https://zero-to-jupyterhub.readthedocs.io/en/latest/jupyterhub/customizing/user-environment.html
FROM pangeo/base-notebook:2020.11.06
USER root
RUN apt-get update --yes && apt-get install --yes git openssh-client
USER jovyan
ENV PATH=/srv/conda/envs/notebook/bin:$PATH
RUN pip install --no-cache-dir rucio-clients
RUN cp /srv/conda/envs/notebook/etc/rucio.cfg.atlas.client.template /srv/conda/envs/notebook/etc/rucio.cfg
RUN pip install --no-cache-dir numpy h5py numba uproot awkward pyarrow coffea aiohttp
RUN pip install --no-cache-dir git+https://gitlab.cern.ch/nihartma/physlite-experiments@e11521d5d67248b5f463087b7afde5cae78b02b8
