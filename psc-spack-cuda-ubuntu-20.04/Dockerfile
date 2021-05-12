
ARG VARIANT="ubuntu-20.04"
FROM ghcr.io/germasch/psc-spack-${VARIANT}:latest

RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin \
    && mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600 \
    && apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/7fa2af80.pub \
    && add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/ /"

# === from https://gitlab.com/nvidia/container-images/cuda/blob/master/dist/11.3.0/ubuntu20.04-x86_64/base/Dockerfile

# For libraries in the cuda-compat-* package: https://docs.nvidia.com/cuda/eula/index.html#attachment-a
RUN apt-get update && apt-get install -y --no-install-recommends \
    cuda-cudart-11-3=11.3.58-1 \
    cuda-compat-11-3 \
    && ln -s cuda-11.3 /usr/local/cuda && \
    rm -rf /var/lib/apt/lists/*

RUN echo "/usr/local/nvidia/lib" >> /etc/ld.so.conf.d/nvidia.conf \
    && echo "/usr/local/nvidia/lib64" >> /etc/ld.so.conf.d/nvidia.conf

ENV PATH /usr/local/nvidia/bin:/usr/local/cuda/bin:${PATH}
ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64

#COPY NGC-DL-CONTAINER-LICENSE /

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
ENV NVIDIA_REQUIRE_CUDA "cuda>=11.3 brand=tesla,driver>=418,driver<419 brand=tesla,driver>=440,driver<441 driver>=450"

# === from https://gitlab.com/nvidia/container-images/cuda/blob/master/dist/11.3.0/ubuntu20.04-x86_64/runtime/Dockerfile

RUN apt-get update && apt-get install -y --no-install-recommends \
    cuda-libraries-11-3=11.3.0-1 \
    libnpp-11-3=11.3.3.44-1 \
    cuda-nvtx-11-3=11.3.58-1 \
    libcublas-11-3=11.4.2.10064-1 \
    libcusparse-11-3=11.5.0.58-1 \
    && rm -rf /var/lib/apt/lists/*

#    libnccl2=$NCCL_VERSION-1+cuda11.3 \

# apt from auto upgrading the cublas package. See https://gitlab.com/nvidia/container-images/cuda/-/issues/88
RUN apt-mark hold libcublas-11-3 #libnccl2

# === from https://gitlab.com/nvidia/container-images/cuda/blob/master/dist/11.3.0/ubuntu20.04-x86_64/devel/Dockerfile

ENV NCCL_VERSION 2.9.6

RUN apt-get update && apt-get install -y --no-install-recommends \
    libtinfo5 libncursesw5 \
    cuda-cudart-dev-11-3=11.3.58-1 \
    cuda-command-line-tools-11-3=11.3.0-1 \
    cuda-minimal-build-11-3=11.3.0-1 \
    cuda-libraries-dev-11-3=11.3.0-1 \
    cuda-nvml-dev-11-3=11.3.58-1 \
    libnpp-dev-11-3=11.3.3.44-1 \
    libnccl-dev=2.9.6-1+cuda11.3 \
    libcublas-dev-11-3=11.4.2.10064-1 \
    libcusparse-dev-11-3=11.5.0.58-1 \
    && rm -rf /var/lib/apt/lists/*

# apt from auto upgrading the cublas package. See https://gitlab.com/nvidia/container-images/cuda/-/issues/88

RUN apt-mark hold libcublas-dev-11-3 libnccl-dev
ENV LIBRARY_PATH /usr/local/cuda/lib64/stubs

RUN echo "  cuda: {externals: [{spec: cuda@11.3.0, prefix: /usr/local}]}" >> .spack/packages.yaml

RUN git clone https://github.com/psc-code/psc \
    && spack repo add psc/spack/psc

RUN spack env activate psc-env && spack add thrust \
    && spack install

