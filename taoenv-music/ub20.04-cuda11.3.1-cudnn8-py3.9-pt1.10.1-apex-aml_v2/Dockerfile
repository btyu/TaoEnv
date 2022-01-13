# Build from basic image: 
FROM registry.cn-shanghai.aliyuncs.com/taons/taoenv-music:ub20.04-cuda11.3.1-cudnn8-py3.7-pt1.10.1-apex-aml


# Install clang and triton 2.0
ENV PATH="$PATH:/usr/local/llvm/bin"
RUN mkdir -p /usr/local \
 && cd /usr/local \
 && wget https://github.com/llvm/llvm-project/releases/download/llvmorg-13.0.0/clang+llvm-13.0.0-x86_64-linux-gnu-ubuntu-20.04.tar.xz \
 && tar xvf clang+llvm-13.0.0-x86_64-linux-gnu-ubuntu-20.04.tar.xz \
 && mv clang+llvm-13.0.0-x86_64-linux-gnu-ubuntu-20.04 llvm \
 && rm -r ./clang+llvm-13.0.0-x86_64-linux-gnu-ubuntu-20.04.tar.xz \
 && cd /root \
 && git clone https://github.com/btyu/triton.git \
 && cd triton \
 && git checkout remotes/origin/v2.0 \
 && cd python \
 && pip install -e .


# Install GPU Python NVML and Other packages
RUN pip install pytorch_fast_transformers

RUN chmod -R 777 /root


 # Set the default command to bash
CMD ["bash"]
