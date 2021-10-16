FROM ubuntu:18.04

# To make it easier for build and release pipelines to run apt-get,
# configure apt to not require confirmation (assume the -y argument by default)
ENV DEBIAN_FRONTEND=noninteractive
RUN echo "APT::Get::Assume-Yes \"true\";" > /etc/apt/apt.conf.d/90assumeyes

RUN apt-get update && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    jq \
    git \
    iputils-ping \
    libcurl4 \
    libicu60 \
    libunwind8 \
    netcat \
    libssl1.0 \
    wget \
    unzip \
    ansible \
    python3-pip \
    && rm -rf /var/lib/apt/lists/*

RUN useradd -m ghactions

RUN ln -sf /usr/bin/python3 /usr/bin/python

COPY requirements.txt requirements.txt

RUN pip3 install -r requirements.txt

ARG TARGETARCH=amd64
ARG RUNNER_VERSION
ARG TF_VERSION

RUN echo ${TF_VERSION}

RUN wget -q https://releases.hashicorp.com/terraform/${TF_VERSION}/terraform_${TF_VERSION}_linux_amd64.zip

RUN unzip terraform_${TF_VERSION}_linux_amd64.zip && mv terraform bin/terraform

RUN apt-get autoclean
RUN rm -rf  terraform_${TF_VERSION}_linux_amd64.zip

RUN cd /home/ghactions && mkdir actions-runner && cd actions-runner \
    && curl -O -L https://github.com/actions/runner/releases/download/v${RUNNER_VERSION}/actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz \
    && tar xzf ./actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz

RUN chown -R ghactions ~ghactions && /home/ghactions/actions-runner/bin/installdependencies.sh

COPY start.sh start.sh

RUN chmod +x start.sh

USER ghactions

RUN rm -rf /home/ghactions/actions-runner/actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz


ENTRYPOINT ["./start.sh"]