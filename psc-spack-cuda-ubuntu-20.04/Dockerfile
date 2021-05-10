
ARG VARIANT="ubuntu-20.04"
FROM ghcr.io/germasch/psc-spack-${VARIANT}:latest

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    libopenmpi-dev ninja-build \
    apt-transport-https ca-certificates gnupg software-properties-common wget \
    libhdf5-openmpi-dev

RUN git clone https://github.com/psc-code/psc \
    && spack repo add psc/spack/psc

RUN spack env activate psc-env && spack add cuda \
    && spack install

RUN spack env activate psc-env && spack add thrust rmm gtensor \
    && spack install

