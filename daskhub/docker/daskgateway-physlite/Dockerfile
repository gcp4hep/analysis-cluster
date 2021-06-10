FROM daskgateway/dask-gateway:0.9.0
USER root
RUN apt-get update --yes && apt-get install --yes git openssh-client
USER 1000:1000
RUN pip install --no-cache-dir numpy h5py numba uproot awkward pyarrow coffea aiohttp
RUN pip install --no-cache-dir git+https://gitlab.cern.ch/nihartma/physlite-experiments@e11521d5d67248b5f463087b7afde5cae78b02b8
RUN pip install --no-cache-dir rucio-clients
