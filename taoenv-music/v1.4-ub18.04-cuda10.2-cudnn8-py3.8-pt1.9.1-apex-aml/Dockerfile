# Build from basic image: 
FROM mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.2-cudnn8-ubuntu18.04

# Set Home Environment
ENV HOME=/root
WORKDIR /root

# Install some basic utilities
RUN apt-get update \
 && apt-get install -y \
    wget gcc libxml2-dev libxslt1-dev zlib1g-dev g++ vim git  cmake  libncurses5-dev libncursesw5-dev screen \
 && apt-get clean -y \
 && rm -rf /var/lib/apt/lists/* \
# nvtop
 && git clone https://github.com/Syllo/nvtop.git \
 && mkdir -p nvtop/build \
 && cd nvtop/build \
 && cmake .. \
 && make \
 && make install \
 && cd ../.. \
 && rm -r nvtop

# Install Miniconda and Python Packages
ENV CONDA_AUTO_UPDATE_CONDA=false
RUN conda init \
 && eval "$('/opt/miniconda/bin/conda' 'shell.bash' 'hook' 2> /dev/null)" \
 && chmod -R 777 /opt \
 && conda clean -i \
 && conda install -y -c pytorch \
    python=3.8 pytorch=1.9.1 cudatoolkit=10.2 tensorboard=2.2.1 jupyter numpy \
 && conda clean -ya \
 && pip install --no-cache-dir \
    tensorboardX transformers==4.3.0 matplotlib pytorch_lightning==1.4.9 py-midi==2.0.1 pretty_midi==0.2.9 pypianoroll==0.4.0 fairseq==0.10.2 pyarrow==4.0.0 miditoolkit==0.1.14

# Install apex
RUN git clone https://github.com/NVIDIA/apex \
 && cd apex \
 && pip install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./ \
 && cd .. && rm -r ./apex

 # Set the default command to bash
CMD ["bash"]
