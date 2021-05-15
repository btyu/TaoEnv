# Build from basic image: taoenv-base:v1.0-ub16.04-cuda10.2-cudnn8-py3.8-pt1.7
FROM registry.cn-shanghai.aliyuncs.com/taons/taoenv-base:v1.0-ub16.04-cuda10.2-cudnn8-py3.8-pt1.7

# Install music related libraries
RUN pip install --no-cache-dir \
    py-midi==2.0.1 pretty_midi==0.2.9 pypianoroll==0.4.0 fairseq==0.10.2 pyarrow==4.0.0 miditoolkit==0.1.14

# Install apex  https://github.com/NVIDIA/apex/issues/1043
RUN git clone https://github.com/NVIDIA/apex \
 && cd apex \
 && git reset --hard 3fe10b5597ba14a748ebb271a6ab97c09c5701ac \
 && pip install -v --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" \
    --global-option="--deprecated_fused_adam" --global-option="--xentropy" \
    --global-option="--fast_multihead_attn" ./ \
 && cd .. \
 && rm -r ./apex

# Set the default command to bash
CMD ["bash"]
