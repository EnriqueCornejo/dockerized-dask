FROM continuumio/miniconda3:4.3.14

# RUN apt-get update --fix-missing && apt-get install -y graphviz graphviz-dev

# Dumb init
RUN wget -O /usr/local/bin/dumb-init https://github.com/Yelp/dumb-init/releases/download/v1.2.0/dumb-init_1.2.0_amd64
RUN chmod +x /usr/local/bin/dumb-init

RUN conda create -n dask --yes -c conda-forge \
    dask distributed numpy scipy pandas numba nomkl fastparquet s3fs zict bcolz blosc cytoolz netCDF4 graphviz matplotlib memory_profiler xarray jupyter_kernel_gateway \
    && conda clean -tipsy
ENV PATH=/opt/conda/envs/dask/bin:$PATH

RUN pip install --upgrade pip
RUN pip install --upgrade google-cloud-storage

COPY base/prepare.sh /usr/bin/prepare.sh
RUN chmod +x /usr/bin/prepare.sh
COPY work/baseline_tasmax_99p.npy /home/jovyan/work/baseline_tasmax_99p.npy

RUN mkdir /opt/app

ENTRYPOINT ["/usr/local/bin/dumb-init", "--"]
CMD ["bash", "-c", "/usr/bin/prepare.sh && exec dask-scheduler"]
