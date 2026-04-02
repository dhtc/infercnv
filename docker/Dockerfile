# Docker file for inferCNV
FROM bioconductor/bioconductor_docker@sha256:3ed828b0c754837dd55bcae8986f73988a2b8c1eff0855b7cbbf92efbddaa42f

RUN printf "# 默认注释了源码镜像以提高 apt update 速度\n\
deb http://mirrors4.tuna.tsinghua.edu.cn/ubuntu/ noble main restricted universe multiverse\n\
deb http://mirrors4.tuna.tsinghua.edu.cn/ubuntu/ noble-updates main restricted universe multiverse\n\
deb http://mirrors4.tuna.tsinghua.edu.cn/ubuntu/ noble-backports main restricted universe multiverse\n\
deb http://security.ubuntu.com/ubuntu/ noble-security main restricted universe multiverse\n" > /etc/apt/sources.list \
    && apt-get update && apt-get -y install curl libssl-dev libcurl4-openssl-dev \
                                        libxml2-dev git python3 jags libssl-dev libxml2-dev cmake python3-pip python3-dev\
                                        r-cran-rjags time && \
                      apt-get clean && rm -rf /var/tmp/* \
                                          /tmp/* /var/lib/apt/lists/*
                                          
# Install R and Bioconductor packages
RUN echo "options(repos = c(CRAN='https://mirrors.aliyun.com/CRAN/', BioC_mirror='https://mirrors.westlake.edu.cn/bioconductor'))" >.Rprofile

RUN pip install --no-cache-dir --break-system-packages \
    -i https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple \
    jupyterlab

RUN R -e "install.packages('IRkernel')" && R -e "IRkernel::installspec(user = FALSE)"

RUN R -e "BiocManager::install(c('infercnv', 'remotes'))"
RUN R -e "remotes::install_github('qsbase/qs', configure_args = '--with-simd=AVX2', force = TRUE, quiet = TRUE)"
RUN R -e "install.packages(c('Seurat'))"
RUN R -e "remotes::install_github('satijalab/seurat-wrappers')"
RUN R -e "install.packages(c('cluster', 'optparse', 'igraph', 'reshape2'))"
#RUN git clone https://gh-proxy.org/https://github.com/broadinstitute/infercnv && cd infercnv && \
#      git checkout master && \
#      R CMD INSTALL . 
#ENV PATH=${PATH}:/infercnv/scripts

RUN R -e 'remotes::install_github("must-bioinfo/fastCNV")'
#RUN R -e 'install.packages("fastCNV", repos = "https://zaoqu-liu.r-universe.dev")'

RUN R -e 'remotes::install_github("XuegongLab/HGC")'


