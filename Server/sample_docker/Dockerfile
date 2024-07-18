# Base image
FROM redhat/ubi8:latest

# Install necessary packages
RUN yum -y update && yum -y install \
    wget \
    bzip2 \
    gcc \
    openssh-server \
    make \
    git \
    sudo && \
    yum clean all

RUN sudo dnf -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
RUN sudo dnf -y install htop

RUN dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm
RUN dnf update -y

# postgresql client version
RUN dnf install -y postgresql14 && \
    dnf clean all 

# Download, extract, and install Redis CLI
RUN wget http://download.redis.io/redis-stable.tar.gz && \
    tar xvzf redis-stable.tar.gz && \
    cd redis-stable && \
    make && \
    cp src/redis-cli /usr/bin/ && \
    cd .. && \
    rm -rf redis-stable.tar.gz redis-stable

# Set environment variables
ENV PATH="/opt/conda/bin:$PATH"

# Install Miniconda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /miniconda.sh && \
    bash /miniconda.sh -b -p /opt/conda && \
    rm /miniconda.sh

# Install Python 
RUN conda create --name dev python=3.10 -y && \
    conda clean --all --yes && \
    conda init bash && \
    echo "source activate dev" > ~/.bashrc

# Copy requirements.txt into the container
COPY requirements.txt /app/requirements.txt

# Install Jupyter Notebook, PyTorch, and packages from requirements.txt
RUN /bin/bash -c "source activate dev && \
    pip install jupyter && \
    pip install torch==1.13.1+cu117 torchvision==0.14.1+cu117 torchaudio==0.13.1 --extra-index-url https://download.pytorch.org/whl/cu117 && \
    pip install -r /app/requirements.txt && \ 
    pip install psycopg2-binary" 

CMD ["tail", "-f", "/dev/null"]