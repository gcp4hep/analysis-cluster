FROM pangeo/base-notebook:2021.09.08

SHELL ["/bin/bash", "-c"]

# machine learning packages
RUN conda install --yes -c conda-forge -n notebook nb_conda_kernels root flatbuffers=1.12 joblib=1.0.1 pillow=8.1.0 pytz=2021.1 scikit-learn=0.24.1 scipy=1.6.0 cudnn=8.2.1

# necessary since root-numpy depends on root package installation
RUN source activate notebook && pip install energyflow root-numpy sklearn==0.0 awkward==1.1.1 uproot==4.0.2 tensorflow==2.6.2 cupy-cuda110 cx_Oracle pyarrow matplotlib mplhep==0.2.18 tables

# gfal2 is necessary for rucio download
RUN conda install --yes -c conda-forge -n notebook python-gfal2 gfal2 gfal2-util

# install rucio clients
ENV PATH=/srv/conda/envs/notebook/bin:$PATH
RUN pip install --no-cache-dir rucio-clients
RUN cp /srv/conda/envs/notebook/etc/rucio.cfg.atlas.client.template /srv/conda/envs/notebook/etc/rucio.cfg

# rucio settings and CA certificates
ENV RUCIO_HOME=/srv/conda/envs/notebook/
ENV X509_CERT_DIR=/cvmfs/atlas.cern.ch/repo/ATLASLocalRootBase/etc/grid-security-emi/certificates

# install git and openssh
USER root
RUN apt-get update --yes && apt-get install --yes git openssh-client
USER jovyan

# environment variables to use the GKE NVIDIA installation
ENV LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/nvidia/lib64
ENV PATH=${PATH}:/usr/local/nvidia/bin