#MAINTAINER Kuldeep Sahu <ksahu@cloudera.com>
#MAINTAINER Avanish Tiwari <aktiwari@cloudera.com>
# Base Image
FROM ubuntu:23.10

ARG DEBIAN_FRONTEND=noninteractive
ARG LATEST_TERRAFORM_VERSION=1.8.5
ENV PIP_BREAK_SYSTEM_PACKAGES 1

# Directory for storing scripts.
RUN mkdir -p /opt/cdp-public-cloud-automation

# Setting Up Environment Variables.
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV HOME_DIR="/opt/cdp-public-cloud-automation"

# Installing Required Packages and Dependencies.
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    jq wget git make unzip curl software-properties-common \
    python3 python3-pip python3-setuptools python3-dev \
    gcc libssl-dev && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean && \
    rm -rf /var/cache/apt/* && \
    \
    # Install Python packages.
    pip3 install --no-cache-dir \
    awscli cdpcli setuptools_rust ansible \
    && \
    \
    # Install latest Terraform version.
    wget -O /tmp/terraform.zip https://releases.hashicorp.com/terraform/${LATEST_TERRAFORM_VERSION}/terraform_${LATEST_TERRAFORM_VERSION}_linux_amd64.zip && \
    unzip /tmp/terraform.zip -d /usr/local/bin/ && \
    rm -rf /tmp/terraform.zip

# Copy necessary files.
COPY cdp-wrkshps-quickstarts /opt/cdp-public-cloud-automation/cdp-wrkshps-quickstarts
COPY entrypoint/docker-entrypoint.sh /usr/local/bin/
COPY entrypoint/hol-functions.sh /usr/local/bin/
COPY entrypoint/default.py /usr/local/lib/python3/dist-packages/ansible/plugins/callback/

# Set execute permission for entrypoint script.
RUN chmod +x /usr/local/bin/docker-entrypoint.sh

# Set working directory.
WORKDIR "$HOME_DIR"

# Set default command.
#CMD ["/bin/bash"]
ENTRYPOINT ["docker-entrypoint.sh"]
