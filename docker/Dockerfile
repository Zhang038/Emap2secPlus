FROM nvidia/cuda:11.1.1-cudnn8-runtime-ubuntu18.04

SHELL ["/bin/bash", "-c"]

RUN apt-get update

RUN apt-get install -y \
        git \
        wget \

# Install Miniconda package manager.
RUN wget -q -P /tmp \
  https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    && bash /tmp/Miniconda3-latest-Linux-x86_64.sh -b -p /opt/conda \
    && rm /tmp/Miniconda3-latest-Linux-x86_64.sh

# Install conda packages.
ENV PATH="/opt/conda/bin:$PATH"
RUN conda update -qy conda \
    && conda install -y -c conda-forge \
      pip \
      python=3.7

COPY . /app/Emap2secPlus

# Install pip packages.
RUN pip3 install --upgrade pip \
    && pip3 install -r /app/Emap2secPlus/requirements.txt

# write a shell script to wrap them up.
WORKDIR /app/Emap2secPlus
RUN echo $'#!/bin/bash\n\
ldconfig\n\
python /app/Emap2secPlus/main.py "$@"' > /app/run_main.sh \
  && chmod +x /app/run_main.sh
ENTRYPOINT ["/app/run_main.sh"]
