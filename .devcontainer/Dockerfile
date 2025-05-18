# Start from the devcontainer Ubuntu 24.04 image
FROM mcr.microsoft.com/devcontainers/base:ubuntu-24.04

ENV DEBIAN_FRONTEND=noninteractive

# 1) Install Docker CLI, Containerd, Docker Compose & Buildx
RUN apt-get update && apt-get install -y \
      ca-certificates \
      curl \
      gnupg \
      lsb-release && \
    install -m 0755 -d /etc/apt/keyrings && \
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg \
      -o /etc/apt/keyrings/docker.asc && \
    chmod a+r /etc/apt/keyrings/docker.asc && \
    echo "deb [arch=$(dpkg --print-architecture) \
      signed-by=/etc/apt/keyrings/docker.asc] \
      https://download.docker.com/linux/ubuntu \
      $(. /etc/os-release && echo \"${UBUNTU_CODENAME:-$VERSION_CODENAME}\") \
      stable" > /etc/apt/sources.list.d/docker.list && \
    apt-get update && \
    apt-get install -y \
      docker-ce \
      docker-ce-cli \
      containerd.io \
      docker-compose-plugin \
      docker-buildx-plugin && \
    rm -rf /var/lib/apt/lists/*

# 2) Install development dependencies
RUN apt-get update && apt-get install -y \
      build-essential \
      pkg-config \
      libudev-dev \
      llvm \
      libclang-dev \
      protobuf-compiler \
      libssl-dev \
      git && \
    rm -rf /var/lib/apt/lists/*

# 3) Switch to the vscode user to install Solana into their home folder
USER vscode

# 4) Install Solana CLI (this also installs Anchor via Solana's tooling)
RUN curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash


RUN mkdir -p /home/vscode/.cargo/bin && \
    curl -L "https://bin.arcium.com/download/arcup_x86_64_linux_0.1.47" \
      -o /home/vscode/.cargo/bin/arcup && \
    chmod +x /home/vscode/.cargo/bin/arcup

# 5) Expose both Solana's and Cargo's bin directories on PATH for vscode
ENV PATH="/home/vscode/.local/share/solana/install/active_release/bin:\
/home/vscode/.cargo/bin:${PATH}"

# 6) Switch back to root
USER root
