FROM spack/ubuntu-bionic

RUN apt-get update && DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends libssl-dev \
    libbz2-dev \
    silversearcher-ag vim less

RUN spack config add "packages:all:target: [haswell]" \
    && spack external find \&& spack config get packages

# RUN git clone https://github.com/psc-code/psc \
#     && spack repo add psc/spack/psc

# RUN spack dev-build -u cmake psc@develop

RUN spack env create -d psc-env

RUN spack env activate psc-env && spack add pkgconf \
    && spack install

RUN spack env activate psc-env && spack add cmake \
    && spack install

RUN spack env activate psc-env && spack add openmpi \
    && spack install

RUN spack env activate psc-env && spack add adios2 \
    && spack install

RUN spack env activate psc-env && spack add hdf5 +hl \
    && spack install

RUN spack env activate psc-env && spack add cuda \
    && spack install

RUN spack env activate psc-env && spack add thrust rmm \
    && spack install
