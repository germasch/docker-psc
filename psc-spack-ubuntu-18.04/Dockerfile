FROM spack/ubuntu-bionic

RUN apt-get update && DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends libssl-dev \
    libbz2-dev

RUN spack external find 

RUN git clone https://github.com/psc-code/psc

RUN spack repo add psc/spack/psc

RUN spack install adios2 