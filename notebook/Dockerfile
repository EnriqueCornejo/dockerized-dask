FROM jupyter/base-notebook

# ENV NB_UID=1004
# ENV NB_GID=1005

USER root
RUN apt-get update --fix-missing && apt-get install -y graphviz graphviz-dev aria2

USER $NB_USER

RUN conda create -n dask --yes -c conda-forge \
    dask distributed numpy scipy pandas numba nomkl fastparquet s3fs zict bcolz blosc cytoolz netCDF4 graphviz python-graphviz matplotlib memory_profiler xarray \
    'notebook=5.0.*' \
    'jupyterhub=0.7.*' \
    'jupyterlab=0.24.*' \
    'python=3.6.3' \
    && conda clean -tipsy
ENV PATH=/opt/conda/envs/dask/bin:$PATH

USER root
COPY ./base/prepare.sh /usr/bin/prepare.sh
RUN chmod +x /usr/bin/prepare.sh

RUN fix-permissions /home/$NB_USER
CMD ["bash", "-c", "/usr/bin/prepare.sh && start-notebook.sh"]
