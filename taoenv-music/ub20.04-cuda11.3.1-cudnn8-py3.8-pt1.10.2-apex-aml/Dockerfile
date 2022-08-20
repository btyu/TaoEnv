# Build from basic image: 
FROM registry.cn-shanghai.aliyuncs.com/taons/aml-base:openmpi4.1.0-cuda11.3.1-cudnn8-ubuntu20.04

# Set Home Environment
ENV HOME=/root
WORKDIR /root
RUN chmod -R 777 /root

# Install some basic utilities
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub \
 && apt-get update \
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
RUN echo 'channels:\n\
  - defaults\n\
show_channel_urls: true\n\
channel_alias: https://mirrors.bfsu.edu.cn/anaconda\n\
default_channels:\n\
  - https://mirrors.bfsu.edu.cn/anaconda/pkgs/main\n\
  - https://mirrors.bfsu.edu.cn/anaconda/pkgs/free\n\
  - https://mirrors.bfsu.edu.cn/anaconda/pkgs/r\n\
  - https://mirrors.bfsu.edu.cn/anaconda/pkgs/pro\n\
  - https://mirrors.bfsu.edu.cn/anaconda/pkgs/msys2\n\
custom_channels:\n\
  conda-forge: https://mirrors.bfsu.edu.cn/anaconda/cloud\n\
  msys2: https://mirrors.bfsu.edu.cn/anaconda/cloud\n\
  bioconda: https://mirrors.bfsu.edu.cn/anaconda/cloud\n\
  menpo: https://mirrors.bfsu.edu.cn/anaconda/cloud\n\
  pytorch: https://mirrors.bfsu.edu.cn/anaconda/cloud\n\
  simpleitk: https://mirrors.bfsu.edu.cn/anaconda/cloud\n' > ~/.condarc
RUN conda init \
 && eval "$('/opt/miniconda/bin/conda' 'shell.bash' 'hook' 2> /dev/null)" \
 && chmod -R 777 /opt/miniconda \
 && conda clean -i \
 && conda install -y -c pytorch \
    python=3.8 pytorch=1.10.2 cudatoolkit=11.3.1 tensorboard jupyter numpy tensorboardX \
 && conda clean -ya \
 && pip install --no-cache-dir \
    transformers==4.10 matplotlib pytorch_lightning==1.4.9 py-midi==2.0.1 pretty_midi==0.2.9 pypianoroll==0.4.0 fairseq==0.10.2 pyarrow==4.0.0 miditoolkit==0.1.14 

# Install apex
RUN git clone https://github.com/NVIDIA/apex \
 && cd apex \
 && pip install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./ \
 && cd .. && rm -r ./apex

 # Set the default command to bash
CMD ["bash"]
