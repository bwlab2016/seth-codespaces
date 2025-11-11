FROM mcr.microsoft.com/devcontainers/base:ubuntu-22.04

# 安装依赖
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get install -y \
    wget \
    curl \
    git \
    build-essential \
    pkg-config \
    libseccomp-dev \
    libbtrfs-dev \
    iptables \
    runc \
    && rm -rf /var/lib/apt/lists/*

# 安装 containerd v2.1.0
ARG CONTAINERD_VERSION=2.1.0
RUN wget https://github.com/containerd/containerd/releases/download/v${CONTAINERD_VERSION}/containerd-${CONTAINERD_VERSION}-linux-amd64.tar.gz \
    && tar Cxzvf /usr/local containerd-${CONTAINERD_VERSION}-linux-amd64.tar.gz \
    && rm containerd-${CONTAINERD_VERSION}-linux-amd64.tar.gz

# 配置 containerd
RUN mkdir -p /etc/containerd \
    && containerd config default > /etc/containerd/config.toml

# 安装 nerdctl (containerd 的 CLI 工具)
ARG NERDCTL_VERSION=1.7.6
RUN wget https://github.com/containerd/nerdctl/releases/download/v${NERDCTL_VERSION}/nerdctl-${NERDCTL_VERSION}-linux-amd64.tar.gz \
    && tar Cxzvf /usr/local/bin nerdctl-${NERDCTL_VERSION}-linux-amd64.tar.gz \
    && rm nerdctl-${NERDCTL_VERSION}-linux-amd64.tar.gz

# 安装 CNI plugins
ARG CNI_VERSION=1.4.0
RUN mkdir -p /opt/cni/bin \
    && wget https://github.com/containernetworking/plugins/releases/download/v${CNI_VERSION}/cni-plugins-linux-amd64-v${CNI_VERSION}.tgz \
    && tar Cxzvf /opt/cni/bin cni-plugins-linux-amd64-v${CNI_VERSION}.tgz \
    && rm cni-plugins-linux-amd64-v${CNI_VERSION}.tgz

# 设置权限
RUN chmod +x /usr/local/bin/* \
    && chmod +x /opt/cni/bin/*