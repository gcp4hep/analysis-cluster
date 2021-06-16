FROM daskgateway/dask-gateway:0.9.0
USER root
RUN apt-get update --yes && apt-get install --yes git openssh-client
USER 1000:1000
RUN pip install --no-cache-dir numpy h5py numba uproot awkward pyarrow coffea aiohttp
RUN pip install --no-cache-dir git+https://gitlab.cern.ch/nihartma/physlite-experiments@f5d092ffdfc03d35a6318f9cc9091d9de3b9fd45
RUN pip install --no-cache-dir rucio-clients
RUN cp /opt/conda/etc/rucio.cfg.atlas.client.template /opt/conda/etc/rucio.cfg
COPY 0001-Bugfix-check-for-file-like-objects-in-from_parquet.patch /opt/conda/lib/python3.8/site-packages/awkward/
WORKDIR /opt/conda/lib/python3.8/site-packages/awkward
RUN patch -p3 < 0001-Bugfix-check-for-file-like-objects-in-from_parquet.patch
WORKDIR /home/dask