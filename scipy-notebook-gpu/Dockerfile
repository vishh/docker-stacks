# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM gcr.io/vishnuk-cloud/minimal-notebook-gpu@sha256:78d0eb50a044cfceba7b1c3094a58a8b4808a269f3ded29bbc7a93159d1ce6c5

MAINTAINER Jupyter Project <jupyter@googlegroups.com>

USER root

# libav-tools for matplotlib anim
RUN apt-get update && \
    apt-get install -y --no-install-recommends libav-tools && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

USER $NB_USER

# Install Python 3 packages
# Remove pyqt and qt pulled in for matplotlib since we're only ever going to
# use notebook-friendly backends in these images
RUN conda install --quiet --yes \
    'nomkl' \
    'ipywidgets=6.0*' \
    'pandas=0.19*' \
    'numexpr=2.6*' \
    'matplotlib=2.0*' \
    'scipy=0.19*' \
    'seaborn=0.7*' \
    'scikit-learn=0.18*' \
    'scikit-image=0.12*' \
    'sympy=1.0*' \
    'cython=0.25*' \
    'patsy=0.4*' \
    'statsmodels=0.8*' \
    'cloudpickle=0.2*' \
    'dill=0.2*' \
    'numba=0.31*' \
    'bokeh=0.12*' \
    'sqlalchemy=1.1*' \
    'hdf5=1.8.17' \
    'h5py=2.6*' \
    'vincent=0.4.*' \
    'beautifulsoup4=4.5.*' \
    'xlrd'  && \
    conda remove --quiet --yes --force qt pyqt && \
    conda clean -tipsy

# Activate ipywidgets extension in the environment that runs the notebook server
RUN jupyter nbextension enable --py widgetsnbextension --sys-prefix

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME /home/$NB_USER/.cache/
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot"

USER $NB_USER
