FROM debian:latest

ARG ANACONDA_URL=https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
ARG USER_NAME
ARG USER_PASSWORD
ARG ROOT_PASSWORD

# INSTALL ANACONDA
RUN apt update && apt upgrade -y && apt autoremove -y && \
apt install sudo curl openssh-client openssh-server -y && \
curl ${ANACONDA_URL} -o ./anaconda.sh && \
bash ./anaconda.sh -b -p /opt/miniconda3 && \
rm ./anaconda.sh

RUN eval "$(/opt/miniconda3/bin/conda shell.bash hook)" && \
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main && \
conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r && \
conda init && \
conda create requests -n default -y && \
conda activate default

# CHANGE CREDENTIALS
USER root
RUN echo ${ROOT_PASSWORD} | passwd root --stdin && \
useradd -m ${USER_NAME} -g sudo && \
echo ${USER_PASSWORD} | passwd ${USER_NAME} --stdin && \
eval "$(/opt/miniconda3/bin/conda shell.bash hook)" && \
conda init

# RUN SSH SERVER, TAKEN FROM https://github.com/masseybradley/openssh-server/
USER ${USER_NAME}
RUN eval "$(/opt/miniconda3/bin/conda shell.bash hook)" && \
conda init
ENTRYPOINT ["/usr/sbin/sshd", "-D", "-e"]