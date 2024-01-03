# trunk-ignore-all(trivy/DS026)
ARG VARIANT="ubuntu-22.04"
# checkov:skip=CKV_DOCKER_2: We do not really need a HEALTHCHECK on this image
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

ARG __WORKSPACE_FOLDER=/workspace

ENV WORKSPACE_FOLDER=${__WORKSPACE_FOLDER}

RUN apt-get update\
    && apt-get -yq install --no-install-recommends python3-pip unzip \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "/tmp/awscliv2.zip" \
    && cd /tmp && unzip awscliv2.zip && ./aws/install && rm "/tmp/awscliv2.zip" \
    && pip install --no-cache-dir\
        cloudformation-cli \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p ${__WORKSPACE_FOLDER} \
    && chown vscode:vscode ${__WORKSPACE_FOLDER}

USER vscode

RUN git config --global --add safe.directory ${__WORKSPACE_FOLDER}

VOLUME [ "${__WORKSPACE_FOLDER}" ]

# Other potential plugind for cfn
        # cloudformation-cli-go-plugin \
        # cloudformation-cli-java-plugin \
        # cloudformation-cli-python-plugin \
        # cloudformation-cli-typescript-plugin \