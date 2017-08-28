# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM gcr.io/vishnuk-cloud/scipy-notebook-gpu

MAINTAINER Jupyter Project <jupyter@googlegroups.com>

USER root

# Install CUDA Profile Tools
RUN pip --no-cache-dir install \
        Pillow \
	h5py \
	ipykernel \
	matplotlib \
	numpy \
	pandas \
	scipy \
	sklearn \
	kubernetes \
	&& \
	python -m ipykernel.kernelspec
	
# Install Python 3 Tensorflow with GPU support
RUN pip install --quiet --no-cache-dir tensorflow-gpu

RUN apt-get update && apt-get install -y pkg-config zip g++ zlib1g-dev unzip libcupti-dev curl apt-transport-https lsb-release openssh-client git
ENV CLOUD_SDK_VERSION 168.0.0
RUN export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)" && \
    echo "deb https://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" > /etc/apt/sources.list.d/google-cloud-sdk.list && \
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - && \
    apt-get update && \
    apt-get install -y google-cloud-sdk=${CLOUD_SDK_VERSION}-0 kubectl && \
    gcloud config set core/disable_usage_reporting true && \
    gcloud config set component_manager/disable_update_check true && \
    gcloud config set metrics/environment github_docker_image
RUN curl -L -O https://github.com/bazelbuild/bazel/releases/download/0.5.4/bazel-0.5.4-installer-linux-x86_64.sh && chmod a+x ./bazel-0.5.4-installer-linux-x86_64.sh && ./bazel-0.5.4-installer-linux-x86_64.sh && rm bazel-0.5.4-installer-linux-x86_64.sh
RUN curl -L -O https://github.com/GoogleCloudPlatform/docker-credential-gcr/releases/download/v1.4.1/docker-credential-gcr_linux_amd64-1.4.1.tar.gz && tar xvzf ./docker-credential-gcr_linux_amd64-1.4.1.tar.gz -C /usr/local/bin/ && rm docker-credential-gcr_linux_amd64-1.4.1.tar.gz
RUN git clone https://github.com/vishh/tf-serving-base.git
# Switch back to jovyan to avoid accidental container runs as root
USER $NB_USER
