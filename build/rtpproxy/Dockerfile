# Use Debian 12 slim as base
FROM debian:12-slim

# Install build dependencies
RUN apt-get update && apt-get install -y \
    ca-certificates \
    net-tools \
    iproute2 \
    sed \
    build-essential \
    git \
    libcurl4-openssl-dev \
    libxml2-dev \
    # perl \
    # perl-modules \
    cpanminus \
    iptables \
    libtool \
    automake \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# # Install Perl Bencode module
# RUN cpanm Net::Bencode

# Clone and build RTPproxy
WORKDIR /usr/src
RUN git clone -b master https://github.com/sippy/rtpproxy.git && \
    cd rtpproxy && \
    git submodule update --init --recursive && \
    ./configure && \
    make && \
    make install && \
    cd .. && \
    rm -rf rtpproxy

# Expose the control port and RTP port range
EXPOSE 8844/udp
EXPOSE 10000-10020/udp

# Set the entrypoint to run RTPproxy with restricted port range
ENTRYPOINT ["rtpproxy", "-l", "0.0.0.0", "-s", "udp:0.0.0.0:8844", "-m", "10000", "-M", "10020", "-fF"]