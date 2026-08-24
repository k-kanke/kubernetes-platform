FROM ubuntu:26.04

LABEL org.opencontainers.image.source="https://github.com/k-kanke/kubernetes-platform"

RUN apt-get update && apt-get install -y \
    git \
    curl \
    ca-certificates \
    build-essential \
    vim \
    && rm -rf /var/lib/apt/lists/*

ARG GO_VERSION=1.27.0
RUN curl -fsSL https://go.dev/dl/go${GO_VERSION}.linux-amd64.tar.gz \
    | tar -C /usr/local -xz

ENV PATH="/usr/local/go/bin:${PATH}"

RUN curl -fsSL https://deb.nodesource.com/setup_24.x | bash - \
    && apt-get install -y nodejs \
    && rm -rf /var/lib/apt/lists/*

RUN npm install -g @openai/codex

RUN git config --system --add safe.directory /workspace
RUN git config --system user.name "k-kanke"
RUN git config --system user.email "kanke.kooooo01@gmail.com"

RUN mkdir -p /root/.ssh \
    && ssh-keyscan github.com >> /root/.ssh/known_hosts

WORKDIR /workspace

CMD ["sleep", "infinity"]
