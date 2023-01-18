# Build from basic image: 
FROM registry.cn-shanghai.aliyuncs.com/taons/taoenv-music:ub20.04-cuda11.3.1-cudnn8-py3.8-pt1.10.2-apex-aml_2


RUN pip uninstall -y fairseq \
 && wget -O fairseq.zip -q  https://github.com/facebookresearch/fairseq/archive/refs/tags/v0.12.2.zip \
 && unzip -q fairseq.zip \
 && cd fairseq-0.12.2 \
 && wget -O replace.py -q "https://down.wss.show/0xvdxc8/a/ab/aabm0xvdxc8?cdn_sign=1674026615-15-0-6491bf434ef6dac635521931aeb61bbd&exp=240&response-content-disposition=attachment%3B%20filename%3D%22replace.py%22%3B%20filename%2A%3Dutf-8%27%27replace.py" \
 && python replace.py \
 && echo "replace done" \
 && pip install . \
 && cd .. \
 && rm -r fairseq-0.12.2 \
 && rm fairseq.zip \
 && pip install --no-cache-dir wandb setuptools==59.5.0

 # Set the default command to bash
CMD ["bash"]
