# Build from basic image: taoenv-music:v1.0-ub16.04-cuda10.2-cudnn8-py3.8-pt1.7
FROM registry.cn-shanghai.aliyuncs.com/taons/taoenv-music:v1.0-ub16.04-cuda10.2-cudnn8-py3.8-pt1.7

# Install apex  https://github.com/NVIDIA/apex/issues/1043
RUN git clone https://github.com/NVIDIA/apex \
 && cd apex \
 && git reset --hard 3fe10b5597ba14a748ebb271a6ab97c09c5701ac \
 && pip install -v --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" \
    --global-option="--deprecated_fused_adam" --global-option="--xentropy" \
    --global-option="--fast_multihead_attn" ./ \
 && rm -r ./apex

# Set the default command to bash
CMD ["bash"]
