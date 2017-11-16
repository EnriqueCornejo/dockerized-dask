FROM jupyter/base-notebook

USER root
RUN apt-get update --fix-missing && apt-get install -y graphviz graphviz-dev

USER $NB_USER

RUN conda create -n dask --yes -c conda-forge \
    dask distributed numpy scipy pandas numba nomkl fastparquet s3fs zict bcolz blosc cytoolz netCDF4 graphviz python-graphviz matplotlib \
    'notebook=5.0.*' \
    'jupyterhub=0.7.*' \
    'jupyterlab=0.24.*' \
    'python=3.6.3' \
    && conda clean -tipsy
ENV PATH=/opt/conda/envs/dask/bin:$PATH

USER root
COPY ./base/prepare.sh /usr/bin/prepare.sh
RUN chmod +x /usr/bin/prepare.sh
RUN mkdir /opt/app
RUN chown -R $NB_USER:root /home/$NB_USER
# RUN fix-permissions /home/$NB_USER

USER $NB_USER

CMD ["bash", "-c", "/usr/bin/prepare.sh && start-notebook.sh"]
