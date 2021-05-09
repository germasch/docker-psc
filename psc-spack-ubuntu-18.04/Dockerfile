FROM spack/ubuntu-bionic

RUN apt-get update && DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends libssl-dev \
    libbz2-dev \
    silversearcher-ag vim less

RUN echo -e "packages:\n  all:\n    target: [haswell]\n" > .spack/packages.yaml \
    && spack external find && cat .spack/packages.yaml

RUN git clone https://github.com/psc-code/psc \
    && spack repo add psc/spack/psc

#RUN spack install adios2 
#RUN spack install lz4

RUN spack dev-build -u cmake psc@develop
