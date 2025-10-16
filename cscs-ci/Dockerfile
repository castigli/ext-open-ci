ARG BASE_IMAGE=docker.io/ubuntu:24.04
FROM ${BASE_IMAGE}

ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8

ARG WORKDIR_PATH=/workspace
ENV WORKDIR=${WORKDIR_PATH}
WORKDIR ${WORKDIR}

ARG DEBIAN_FRONTEND=noninteractive
ARG EXTRA_APTGET=""
RUN apt-get update -qq && apt-get install -qq -y --no-install-recommends \
    build-essential \
    tar \
    wget \
    curl \
    ca-certificates \
    jq \
    git \
    clang \
    ninja-build \
    cmake \
    ${EXTRA_APTGET} && \
    apt-get clean && \
    apt-get autoremove -y && \
    rm -rf /var/lib/apt/lists/*

# Install uv
ARG UV_VERSION=0.9.3
ADD https://astral.sh/uv/${UV_VERSION}/install.sh /uv-installer.sh
RUN UV_UNMANAGED_INSTALL="/opt/uv" sh /uv-installer.sh && rm /uv-installer.sh
ENV PATH="/opt/uv:$PATH"

# Create and activate a python venv
ARG PY_VERSION=3.12
ENV VIRTUAL_ENV=/opt/venv
RUN uv venv -p ${PY_VERSION} --link-mode copy ${VIRTUAL_ENV}
ENV PATH="${VIRTUAL_ENV}/bin:$PATH"
RUN uv pip install --compile-bytecode setuptools wheel pip nanobind
