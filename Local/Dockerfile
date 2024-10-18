# Base image - redhat/ubi8:8.6
FROM redhat/ubi8:8.6

# Set timezone
ENV TZ Asia/Seoul

ARG VENV_NAME
ARG REQUIREMENTS_PATH

# Install necessary packages
RUN yum -y update && yum -y install \
    wget \
    bzip2 \
    gcc \
    nano \
    openssh-server \
    make \
    git \
    sudo && \
    yum clean all

# Install htop
RUN sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
RUN sudo dnf -y install htop

#RUN dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm
#RUN dnf update -y

## postgresql client version
#RUN dnf install -y postgresql14 && \
#    dnf clean all 

# Install Redis CLI
RUN wget http://download.redis.io/redis-stable.tar.gz && \
    tar xvzf redis-stable.tar.gz && \
    cd redis-stable && \
    make && \
    cp src/redis-cli /usr/bin/ && \
    cd .. && \
    rm -rf redis-stable.tar.gz redis-stable

# Set conda environment variables
ENV PATH="/opt/conda/bin:$PATH"

# Install Miniconda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /miniconda.sh && \
    bash /miniconda.sh -b -p /opt/conda && \
    rm /miniconda.sh

# Install Python 
RUN conda config --add channels conda-forge && \
    conda config --set channel_priority strict && \ 
    conda create --name ${VENV_NAME} python=3.10.5 -y && \
    conda clean --all --yes && \
    conda init bash && \
    echo "source activate ${VENV_NAME}" > ~/.bashrc

# Copy requirements.txt into the container
COPY ./requirements.txt ${REQUIREMENTS_PATH}


# Install Jupyter Notebook, PyTorch, and packages from requirements.txt
RUN /bin/bash -c "source activate ${VENV_NAME} && \
    pip install -r ${REQUIREMENTS_PATH}"

CMD ["tail", "-f", "/dev/null"]
