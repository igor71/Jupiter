# We will use latest Ubuntu docker image
FROM ubuntu:latest

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

MAINTAINER Igor Rabkin <igor.rabkin@xiaoyi.com>

# Updating Ubuntu && Installing packages
RUN apt-get update && apt-get install -y emacs wget bzip2 sudo curl grep sed dpkg nano

# Add user ubuntu with no password, add to sudo group
RUN adduser --disabled-password --gecos '' ubuntu && \
    sed -i '23 a ubuntu  ALL=(ALL)  NOPASSWD: ALL' /etc/sudoers

# Install TINI
RUN  TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
     curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
     dpkg -i tini.deb && \
     rm tini.deb && \
     apt-get clean

ENTRYPOINT [ "/usr/bin/tini", "--" ]

# Adding ubuntu user
USER ubuntu
WORKDIR /home/ubuntu/
RUN chmod a+rwx /home/ubuntu/ && \
    mkdir -p /home/ubuntu/.conda

# Anaconda installing
RUN wget https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh && \
    bash Anaconda3-2019.07-Linux-x86_64.sh -b && \
    rm Anaconda3-2019.07-Linux-x86_64.sh

# Set path to conda
ENV PATH /home/ubuntu/anaconda3/bin:$PATH

# Updating Anaconda packages
RUN conda update conda && \
    conda update anaconda  && \
    conda update --all

# Configuring access to Jupyter
RUN mkdir /home/ubuntu/notebooks && \
    jupyter notebook --generate-config --allow-root && \
    echo "c.NotebookApp.password = u'sha1:6a3f528eec40:6e896b6e4828f525a6e20e5411cd1c8075d68619'" >> /home/ubuntu/.jupyter/jupyter_notebook_config.py

# Jupyter listens port: 8889
EXPOSE 8889

# Run Jupytewr notebook as Docker main process
CMD ["jupyter", "notebook", "--allow-root", "--notebook-dir=/home/ubuntu/notebooks", "--ip='*'", "--port=8889", "--no-browser"]
