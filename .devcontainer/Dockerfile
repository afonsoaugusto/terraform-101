#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

# Update the VARIANT arg in devcontainer.json to pick an Debian version: buster (or debian-10), stretch (or debian-9)
# To fully customize the contents of this image, use the following Dockerfile instead:
# https://github.com/microsoft/vscode-dev-containers/tree/v0.128.0/containers/debian/.devcontainer/base.Dockerfile
ARG VARIANT="buster"
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

ARG TERRAFORM_VERSION
ENV TERRAFORM_VERSION=0.12.29
ARG TFSEC_VERSION
ENV TFSEC_VERSION=0.24.1

# ** [Optional] Uncomment this section to install additional packages. **
#
RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive && \
    apt-get install -y unzip jq python3-pip

RUN curl https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip -o "terraform_${TERRAFORM_VERSION}_linux_amd64.zip"  && \
    unzip terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    rm -rf terraform_${TERRAFORM_VERSION}_linux_amd64.zip && \
    mv  terraform /bin/terraform && \
    terraform -install-autocomplete && \
    curl -L "$(curl -Ls https://api.github.com/repos/terraform-linters/tflint/releases/latest | grep -o -E "https://.+?_linux_amd64.zip")" -o tflint.zip && \
    unzip tflint.zip && \
    rm tflint.zip && \
    mv tflint /bin/tflint && \
    curl -sLO https://raw.githubusercontent.com/antonbabenko/terraform-cost-estimation/master/terraform.jq && \
    curl -sLO https://github.com/liamg/tfsec/releases/download/v${TFSEC_VERSION}/tfsec-linux-amd64 && \
    mv tfsec-linux-amd64 /bin/tfsec && \
    chmod +x /bin/tfsec && \
    pip3 install terraform-compliance && \
    pip3 install checkov



# https://github.com/terraform-linters/tflint
# https://github.com/antonbabenko/terraform-cost-estimation
# https://github.com/liamg/tfsec
# https://terraform-compliance.com/
# https://www.checkov.io/1.Introduction/Getting%20Started.html
# https://www.one-tab.com/page/h-WRQVAsSgeCbZC95cODlA