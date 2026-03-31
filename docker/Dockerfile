# Docker file for inferCNV
FROM bioconductor/bioconductor_docker@sha256:3ed828b0c754837dd55bcae8986f73988a2b8c1eff0855b7cbbf92efbddaa42f

RUN apt-get update && apt-get -y install curl libssl-dev libcurl4-openssl-dev \
                                        libxml2-dev git python3 jags \
                                        r-cran-rjags time && \
                      apt-get clean && rm -rf /var/tmp/* \
                                          /tmp/* /var/lib/apt/lists/*
                                          
# Install R and Bioconductor packages
RUN echo "options(repos = c(CRAN='https://mirrors.tuna.tsinghua.edu.cn/CRAN/', BioC_mirror='https://mirrors.tuna.tsinghua.edu.cn/bioconductor'))" >.Rprofile
RUN R -e "BiocManager::install(c('infercnv', 'remotes', 'qs', 'arrow'))"

RUN set -e \
    && R -e "install.packages(c('Seurat','BPCells', 'presto', 'glmGamPoi'))" \
    && R -e "remotes::install_github('satijalab/seurat-wrappers', quiet = TRUE)" \
    && R -e "remotes::install_github('mojaveazure/seurat-disk')" \
    && R -e "install.packages(c('bit64', \
                                'sleepwalk', \
                                'scBio', \
                                'qqconf', \
                                'metap'))" \
    && R -e "BiocManager::install(c('ggbio', 'scry','glmGamPoi'))" \
# Wrapper (LIGER, Harmony, conos, CoGAPS, GLM-PCA, fastMNN (batchelor), ALRA (rsvd), Nebulosa, CIPR, miQC, tricycle)
    && R -e "install.packages(c('glmpca','harmony','rliger','rsvd'))" \
    && R -e "BiocManager::install(c('batchelor', 'Nebulosa'))" 

RUN R -e "install.packages(c('cluster', 'optparse', 'igraph', 'reshape2'))"

#RUN git clone https://gh-proxy.org/https://github.com/broadinstitute/infercnv && cd infercnv && \
#      git checkout master && \
#      R CMD INSTALL . 

ENV PATH=${PATH}:/infercnv/scripts

RUN R -e 'remotes::install_github("must-bioinfo/fastCNV")'
#RUN R -e 'install.packages("fastCNV", repos = "https://zaoqu-liu.r-universe.dev")'

CMD echo 'done'

