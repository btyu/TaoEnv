# Build from basic image: Ubuntu 16.04 with CUDA 10.2
FROM nvidia/cuda:10.2-cudnn8-devel-ubuntu16.04

# Set localization to Chinese
ENV LANG C.UTF-8

# Time zone
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

# Change Ubuntu sources and Remove Nvidia & Cuda sources lists to enable apt-get update & Install some basic utilities
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial main\n\
 deb-src http://mirrors.aliyun.com/ubuntu/ xenial main\n\
 \n\
 deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main\n\
 deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main\n\
 \n\
 deb http://mirrors.aliyun.com/ubuntu/ xenial universe\n\
 deb-src http://mirrors.aliyun.com/ubuntu/ xenial universe\n\
 deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe\n\
 deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates universe\n\
 \n\
 deb http://mirrors.aliyun.com/ubuntu/ xenial-security main\n\
 deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main\n\
 deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe\n\
 deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security universe\n' > /etc/apt/sources.list \
 && mv /etc/apt/sources.list.d /etc/apt/sources.list.d.backup \
 && apt-get update \
 && apt-get install -y \
    wget gcc libxml2-dev libxslt1-dev zlib1g-dev g++ vim git  cmake  libncurses5-dev  libncursesw5-dev \
 && rm -rf /var/lib/apt/lists/*

# Set Home Environment
ENV HOME=/root
WORKDIR /root

# nvtop
RUN git clone https://github.com/Syllo/nvtop.git \
 && mkdir -p nvtop/build \
 && cd nvtop/build \
 && cmake .. \
 && make \
 && make install \
 && cd ../.. \
 && rm -r nvtop

# Install Miniconda
ENV CONDA_AUTO_UPDATE_CONDA=false
ENV PATH=/opt/miniconda/bin:$PATH
RUN mkdir -p /opt && chmod -R 777 /opt \
 && echo 'channels:\n\
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
  simpleitk: https://mirrors.bfsu.edu.cn/anaconda/cloud\n' > ~/.condarc \
 && wget -O ~/miniconda.sh https://mirrors.bfsu.edu.cn/anaconda/miniconda/Miniconda3-4.7.12-Linux-x86_64.sh --no-check-certificate \
 && chmod +x ~/miniconda.sh \
 && ~/miniconda.sh -b -p /opt/miniconda \
 && rm ~/miniconda.sh \
 && eval "$(/opt/miniconda/bin/conda shell.bash hook)" \
 && conda init

# Install Python PyTorch and corresponding packages
RUN conda clean -i \
 && conda install -y -c pytorch \
    python=3.9 pytorch=1.9.1 cudatoolkit=10.2 tensorboard=2.2.1 jupyter numpy \
 && conda clean -ya

# Install other required libraries
RUN pip config set global.index-url https://mirrors.bfsu.edu.cn/pypi/web/simple
RUN pip install --no-cache-dir \
    transformers==4.3.0 matplotlib pytorch_lightning==1.4.9

# Install apex 
RUN git clone https://github.com/NVIDIA/apex \
 && cd apex \
 && pip install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./ \
 && cd .. && rm -r ./apex

RUN chmod -R 777 /opt/miniconda

# Set the default command to bash
CMD ["bash"]
