ARG UBUNTU_VERSION=18.04

FROM ubuntu:$UBUNTU_VERSION

RUN set -ex; \
    fetchDeps="pkg-config zip g++ zlib1g-dev unzip wget git"; \
    apt-get update && apt-get install -y --no-install-recommends $fetchDeps; \
    rm -rf /var/lib/apt/lists/*

ARG PY_VERSION=3
RUN set -ex; \
    fetchDeps="python$PY_VERSION python$PY_VERSION-dev python$PY_VERSION-pip"; \
    apt-get update && apt-get install -y --no-install-recommends $fetchDeps; \
    ln -s /usr/bin/python$PY_VERSION /usr/bin/python; \
    rm -rf /var/lib/apt/lists/*

RUN set -ex; \
    pip3 --no-cache-dir install --upgrade pip setuptools

# install the requeriments
# RUN set -ex; \
#    pip3 --no-cache-dir install wheel

COPY docker/requirements.txt /tmp/
RUN set -ex; \
    pip3 --no-cache-dir install --requirement /tmp/requirements.txt
COPY . /tmp/

ENV SHELL=/bin/bash
ENV APP_HOME=/home/workspace
WORKDIR $APP_HOME

# ENV PYTHONUNBUFFERED=0

ARG USERNAME=finopt
ARG USER_UID=1000
ARG USER_GID=$USER_UID
# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

# ********************************************************
# * Anything else you want to do like clean up goes here *
# ********************************************************
# [Optional] Set the default user. Omit if you want to keep the default as root.
USER $USERNAME

# Expose the port for jupyter
EXPOSE 8888

# # Comand for jupyter
CMD ["/bin/bash", "-c", "jupyter-lab --allow-root --no-browser --ip=0.0.0.0"]
