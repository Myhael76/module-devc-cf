ARG VARIANT="ubuntu-22.04"
# checkov:skip=CKV_DOCKER_2: We do not really need a HEALTHCHECK on this image
FROM mcr.microsoft.com/vscode/devcontainers/base:0-${VARIANT}

ARG __WORKSPACE_FOLDER=/workspace

ENV WORKSPACE_FOLDER=${__WORKSPACE_FOLDER}

RUN apt-get -q update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -yq install --no-install-recommends \
        gnupg \
        apt-transport-https \
        ca-certificates \
        graphviz \
        lsb-release \
        python3-diagrams \
        python3-pip \
        ruby \
        unzip \
        wget \
    && gem install \
        cfn-nag \
    && pip install --no-cache-dir\
        cfn-lint \
        cfripper \
        cloudformation-cli \
        pydot \    
    && wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null \
    && echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | tee -a /etc/apt/sources.list.d/trivy.list \
    && apt-get -q update \
    && apt-get -yq install trivy \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "/tmp/awscliv2.zip" \
    && cd /tmp && unzip awscliv2.zip && ./aws/install && rm "/tmp/awscliv2.zip" \
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

# tried to fix trunk but still having problems
# && curl https://get.trunk.io -fsSL | bash \
