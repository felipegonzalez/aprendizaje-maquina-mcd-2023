FROM rocker/rstudio:4.3.0

ENV CRAN=https://packagemanager.posit.co/cran/__linux__/jammy/2023-06-14
ENV LANG=en_US.UTF-8

ENV WORKON_HOME /opt/virtualenvs
ENV PYTHON_VENV_PATH $WORKON_HOME/am2023_env

COPY scripts /rocker_scripts

RUN /rocker_scripts/install_tidyverse.sh

ENV CTAN_REPO=https://www.texlive.info/tlnet-archive/2023/06/15/tlnet
ENV PATH=$PATH:/usr/local/texlive/bin/linux

RUN /rocker_scripts/install_verse.sh

RUN apt-get update && apt-get install -y pngquant

RUN apt-get update && apt-get install -y --no-install-recommends \
        python3-dev \
        python3-venv \
        python3-pip && \
    rm -rf /var/lib/apt/lists/*

## Prepara environment de python
RUN python3 -m venv ${PYTHON_VENV_PATH}
RUN chown -R rstudio:rstudio ${WORKON_HOME}
ENV PATH ${PYTHON_VENV_PATH}/bin:${PATH}
RUN echo "PATH=${PATH}" >> /usr/local/lib/R/etc/Renviron && \
    echo "WORKON_HOME=${WORKON_HOME}" >> /usr/local/lib/R/etc/Renviron && \
    echo "RETICULATE_PYTHON_ENV=${PYTHON_VENV_PATH}" >> /usr/local/lib/R/etc/Renviron

## Because reticulate hardwires these PATHs
RUN ln -s ${PYTHON_VENV_PATH}/bin/pip /usr/local/bin/pip && \
    ln -s ${PYTHON_VENV_PATH}/bin/virtualenv /usr/local/bin/virtualenv
RUN chmod -R a+x ${PYTHON_VENV_PATH}

RUN .${PYTHON_VENV_PATH}/bin/activate && \
    pip install radian Pillow requests scipy pandas\
    tensorflow==2.10.0 keras==2.10.0
  
RUN install2.r --error Rcpp
RUN install2.r --error tidymodels embed textrecipes workflowsets bonsai iml pdp
RUN install2.r --error \
    abind splines2 kableExtra gt\
    glmnet quantreg xgboost\
    ranger baguette rpart.plot\
    doParallel doFuture \
    kernlab kknn\
    tsne irlba\
    ggrepel gganimate patchwork imager

RUN install2.r reticulate tensorflow keras \
    abind RcppRoll
RUN install2.r gifski languageserver


