# Build from basic image: taoenv-base:v1.0-ub16.04-cuda10.2-cudnn8-py3.8-pt1.7
FROM registry.cn-shanghai.aliyuncs.com/taons/taoenv-base:v1.0-ub16.04-cuda10.2-cudnn8-py3.8-pt1.7

# Install music related libraries
RUN pip install --no-cache-dir \
    py-midi==2.0.1 pretty_midi==0.2.9 pypianoroll==0.4.0 pyarrow

# Install fairseq
RUN git clone https://github.com/pytorch/fairseq \
 && cd fairseq \
 && pip install --editable ./ \
 && cd .. \
 && rm -r ./fairseq

# Set the default command to bash
CMD ["bash"]
