FROM ubuntu:22.04

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common \
    iputils-ping \
    libcap2-bin \
    gnupg

RUN install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc && \
    chmod a+r /etc/apt/keyrings/docker.asc && \
    echo \
      "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
      tee /etc/apt/sources.list.d/docker.list > /dev/null

RUN apt-get update && apt-get install -y netcat sudo docker-ce-cli iproute2 wget nano python3 postgresql-client screen

RUN useradd -ms /bin/bash user && usermod -aG sudo user && echo "user     ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN sed -i "s/#force_color_prompt=yes/force_color_prompt=yes/g" /root/.bashrc

ENV DIVE_VERSION="0.12.0"

RUN wget https://github.com/wagoodman/dive/releases/download/v${DIVE_VERSION}/dive_${DIVE_VERSION}_linux_amd64.deb && \
    apt-get install -y ./dive_${DIVE_VERSION}_linux_amd64.deb && \
    rm dive_${DIVE_VERSION}_linux_amd64.deb

RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/kubectl

RUN apt-get install bash-completion && \
    mkdir -p /etc/bash_completion.d/ && \
    kubectl completion bash >>/etc/bash_completion.d/kubectl

RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
    apt-get update && apt-get install -y google-cloud-cli kubectl

ENV KUBECTX_VERSION="0.9.5"

RUN wget https://github.com/ahmetb/kubectx/releases/download/v${KUBECTX_VERSION}/kubectx_v${KUBECTX_VERSION}_linux_x86_64.tar.gz && \
    tar xzf kubectx_v${KUBECTX_VERSION}_linux_x86_64.tar.gz && \
    rm kubectx_v${KUBECTX_VERSION}_linux_x86_64.tar.gz && \
    chown root:root kubectx && \
    mv kubectx /usr/local/bin/. && \
    wget https://github.com/ahmetb/kubectx/releases/download/v${KUBECTX_VERSION}/kubens_v${KUBECTX_VERSION}_linux_x86_64.tar.gz && \
    tar xzf kubens_v${KUBECTX_VERSION}_linux_x86_64.tar.gz && \
    rm kubens_v${KUBECTX_VERSION}_linux_x86_64.tar.gz && \
    chown root:root kubens && \
    mv kubens /usr/local/bin/.

ENV STEP_CLI_VERSION="0.26.1"

RUN wget https://dl.smallstep.com/gh-release/cli/gh-release-header/v${STEP_CLI_VERSION}/step-cli_${STEP_CLI_VERSION}_amd64.deb && \
    dpkg -i step-cli_${STEP_CLI_VERSION}_amd64.deb && \
    rm -f step-cli_${STEP_CLI_VERSION}_amd64.deb

USER 1000

RUN wget -P ~/.nano/syntax https://raw.githubusercontent.com/mbentley/Dockerfile.nanorc/master/Dockerfile.nanorc && \
    echo 'include "~/.nano/syntax/Dockerfile.nanorc"' >> ~/.nanorc

WORKDIR /home/user

RUN sed -i "s/#force_color_prompt=yes/force_color_prompt=yes/g" /home/user/.bashrc
RUN echo 'alias k=kubectl' >> /home/user/.bashrc

ENV TERM xterm-256color

CMD [ "python3", "-m", "http.server" ]
