# Documentation: https://zero-to-jupyterhub.readthedocs.io/en/latest/jupyterhub/customizing/user-environment.html
FROM pangeo/base-notebook:2020.11.06

SHELL ["/bin/bash", "-c"]

RUN conda install --yes -c conda-forge -n notebook nb_conda_kernels root keras=2.4.3 flatbuffers=1.12 joblib=1.0.1 pillow=8.1.0 pytz=2021.1 scikit-learn=0.24.1 scipy=1.6.0

# necessary since root-numpy depends on root package installation
RUN source activate notebook && pip install energyflow root-numpy sklearn==0.0 awkward==1.1.1 uproot==4.0.2

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