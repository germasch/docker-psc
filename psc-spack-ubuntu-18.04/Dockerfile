FROM spack/ubuntu-bionic

RUN apt-get update && DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends libssl-dev \
    libbz2-dev \
    silversearcher-ag vim less

RUN spack config add "packages:all:target: [haswell]" \
    && spack external find \&& spack config get packages

# RUN echo -e "packages:\n  all:\n    target: [haswell]\n" > .spack/packages.yaml \
#     && spack external find && cat .spack/packages.yaml

# RUN git clone https://github.com/psc-code/psc \
#     && spack repo add psc/spack/psc

#RUN spack install adios2 
#RUN spack install lz4

#RUN spack dev-build -u cmake psc@develop

RUN spack env create -d psc-env \
    && spack env activate psc-env \
    && spack add openmpi \
    && spack install

RUN spack env activate psc-env && spack add pkgconf \
    && spack install

RUN spack env activate psc-env && spack add cmake \
    && spack install
