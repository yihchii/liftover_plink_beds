FROM ubuntu:24.04
ARG BCFTOOLS_VER="1.20"
ARG PICARD_VER="3.1.1"
ARG INSTALL_DIR="/usr/local"
ARG PLINK1_VER="20231211"
ARG PLINK2_VER="20240526"

RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    build-essential \
    openjdk-21-jre-headless \
    unzip \
    liblz4-dev \
    wget \
    zlib1g-dev \
 && apt-get autoclean && apt-get autoremove && rm -rf /var/lib/apt/lists/*

WORKDIR /home/root
RUN wget -O - https://github.com/samtools/bcftools/releases/download/${BCFTOOLS_VER}/bcftools-${BCFTOOLS_VER}.tar.bz2 \
  | tar xj \
  && (cd bcftools-${BCFTOOLS_VER} && ./configure --disable-bz2 --disable-lzma --prefix=${INSTALL_DIR} && make && make install) \
  && rm -rf /home/root/bcftools-${BCFTOOLS_VER}

RUN mkdir -p /opt/picard/ \
  && wget https://github.com/broadinstitute/picard/releases/download/${PICARD_VER}/picard.jar \
  && mv picard.jar /opt/picard/

RUN wget https://s3.amazonaws.com/plink2-assets/plink2_linux_avx2_${PLINK2_VER}.zip \
  && unzip plink2_linux_avx2_${PLINK2_VER}.zip && mv plink2 "${INSTALL_DIR}/bin" \
  && rm -rf plink2_linux_avx2_${PLINK2_VER}.zip

RUN wget https://s3.amazonaws.com/plink1-assets/plink_linux_x86_64_${PLINK1_VER}.zip \
  && unzip plink_linux_x86_64_${PLINK1_VER}.zip && mv plink "${INSTALL_DIR}/bin" && mv prettify "${INSTALL_DIR}/bin" \
  && rm -rf plink_linux_x86_64_${PLINK1_VER}.zip toy.map toy.ped