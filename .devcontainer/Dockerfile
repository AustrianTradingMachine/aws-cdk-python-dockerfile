# Dockerfile for AWS CDK Python 3.9 development environment
# Amazon Linux 2023 standard python version is 3.9
# https://docs.aws.amazon.com/linux/al2023/ug/python.html
#
#------------------------------------------------
# Select the base image
#------------------------------------------------
#
# Use the official Debian image as the base image
# https://hub.docker.com/_/debian
# Debian Bullseye (11) used which also has python 3.9 as standard version
FROM debian:11
#
#------------------------------------------------
# Set the metadata
#------------------------------------------------
#
LABEL name="aws-cdk-python3.9"
LABEL version="0.0.1"
LABEL description="Dockerfile for AWS CDK Python 3.9 development environment"
#
#------------------------------------------------
# Define arguments for the base image
#------------------------------------------------
#
# Define the architecture argument
ARG TARGETARCH
#
# Prevent dialog during apt install
ARG DEBIAN_FRONTEND noninteractive
#
#------------------------------------------------
# Install apt packages
#------------------------------------------------
#
# Update the repository sources list
# Clean up the apt-get cache
RUN apt-get update --fix-missing && \
    apt-get install -y -q --no-install-recommends \
        software-properties-common \
        apt-transport-https \
        build-essential \
        ca-certificates \
        sudo \
        curl \
        unzip \
        git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
#
#------------------------------------------------
# Install NPM
#------------------------------------------------
#
# nvm environment variables
ENV NVM_DIR /usr/local/nvm
ENV NODE_VERSION v20.16.0
#
# Create nvm directory
# Install nvm
RUN mkdir -p $NVM_DIR && \
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash && \
    . $NVM_DIR/nvm.sh && \
    nvm install $NODE_VERSION && \
    nvm use $NODE_VERSION && \
    nvm alias default $NODE_VERSION
#
# Add nvm.sh to .bashrc
ENV NODE_PATH $NVM_DIR/$NODE_VERSION/lib/node_modules
ENV PATH $NVM_DIR/versions/node/$NODE_VERSION/bin:$PATH
#
#------------------------------------------------
# Install AWS CLI and AWS CDK
#------------------------------------------------
#
# Install AWS CLI
RUN if [ "$TARGETARCH" = "amd64" ]; then \
        echo "Installing packages for amd64" && \
        curl https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip -o awscliv2.zip; \
    elif [ "$TARGETARCH" = "arm64" ]; then \
        echo "Installing packages for arm64" && \
        curl https://awscli.amazonaws.com/awscli-exe-linux-aarch64.zip -o awscliv2.zip; \
    fi && \
    unzip awscliv2.zip && \
    ./aws/install && \
    rm -rf awscliv2.zip aws
#
# Install AWS CDK
RUN npm install -g aws-cdk
#
#------------------------------------------------
# Install Python 3.9
#------------------------------------------------
#
# Install Python 3.9 with pip, venv, dev, setuptools, wheel
# Clean up the apt-get cache
RUN apt-get update --fix-missing && \
    apt-get install -y -q --no-install-recommends \
        python3 \
        python3-pip \
        python3-venv \
        python3-dev \
        python3-setuptools \
        python3-wheel && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
#
#------------------------------------------------
# Set the working directory
#------------------------------------------------
#
# Create a directory for the app
RUN mkdir -p /workspaces
#
# Set the working directory
WORKDIR /workspaces
#
# Set the working directory as a volume
#VOLUME /workspaces
#
# Copy the current directory contents into the container at /usr/app
COPY .. .
#
#------------------------------------------------
# Install Python dependencies
#------------------------------------------------
#
# Install from reqirements.txt
RUN pip install --no-cache-dir -r requirements.txt || true
#
# Install from requirements-dev.txt
RUN pip install --no-cache-dir -r requirements_dev.txt || true
#
# make sure all messages always reach console
ENV PYTHONUNBUFFERED=1
#
# Add the workspace directory to the PYTHONPATH
ENV PYTHONPATH="${PYTHONPATH}:/workspaces"
#
#------------------------------------------------
# Create a non-root user
#------------------------------------------------
#
# Create a non-root user
RUN groupadd -r developer && useradd -r -g developer -ms /bin/bash developer && \
    mkdir -p /etc/sudoers.d && \
    echo "developer ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/developer
#
# Switch to the new user
USER developer
