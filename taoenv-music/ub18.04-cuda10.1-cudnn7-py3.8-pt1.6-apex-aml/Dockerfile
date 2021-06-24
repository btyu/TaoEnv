# Build from basic image: 
FROM mcr.microsoft.com/azureml/openmpi3.1.2-cuda10.1-cudnn7-ubuntu18.04

# Time zone
RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai' >/etc/timezone

# Set Home Environment
ENV HOME=/root
WORKDIR /root

RUN apt-get update \
 && apt-get install -y --no-install-recommends apt-utils \
 && apt-get install -y vim screen

# Install Miniconda
ENV CONDA_AUTO_UPDATE_CONDA=false

RUN conda init \
 && eval "$('/opt/miniconda/bin/conda' 'shell.bash' 'hook' 2> /dev/null)" \
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
 && conda clean -i \
 && conda install -y -c pytorch \
    python=3.8.3 pytorch=1.6.0 cudatoolkit=10.1 tensorboard=2.2.1 \
 && conda clean -ya \
 && pip install --no-cache-dir \
    transformers==4.3.0 py-midi==2.0.1 pretty_midi==0.2.9 pypianoroll==0.4.0 fairseq==0.10.2 pyarrow==4.0.0 miditoolkit==0.1.14 tensorboardx==2.2

# Install apex  https://github.com/NVIDIA/apex/issues/1043
RUN git clone https://github.com/NVIDIA/apex \
 && cd apex \
 && git reset --hard 3fe10b5597ba14a748ebb271a6ab97c09c5701ac \
 && pip install -v --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" \
    --global-option="--deprecated_fused_adam" --global-option="--xentropy" \
    --global-option="--fast_multihead_attn" ./ \
 && cd .. \
 && rm -rf ./apex
