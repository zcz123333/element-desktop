# Docker image to facilitate building Element Desktop's native bits using a glibc version (2.31)
# with broader compatibility, down to Debian bullseye & Ubuntu focal.
FROM rust:bullseye@sha256:af1a29a166198e1295ca667007e95d2e70c866e3928ba9b25f3907035581c39e

ENV DEBIAN_FRONTEND=noninteractive

RUN curl --proto "=https" -L https://yarnpkg.com/latest.tar.gz | tar xvz && mv yarn-* /yarn && ln -s /yarn/bin/yarn /usr/bin/yarn
RUN apt-get -qq update && apt-get -y -qq dist-upgrade && \
  apt-get -y -qq install --no-install-recommends \
  # tclsh is required for building SQLite as part of SQLCipher
  tcl \
  # libsecret-1-dev is required even for prebuild keytar
  libsecret-1-dev \
  # Used by seshat (when not SQLCIPHER_STATIC) \
  libsqlcipher-dev && \
  apt-get purge -y --auto-remove && rm -rf /var/lib/apt/lists/*
RUN ln -s /usr/bin/python3 /usr/bin/python & ln -s /usr/bin/pip3 /usr/bin/pip

ENV DEBUG_COLORS=true
ENV FORCE_COLOR=true

WORKDIR /project

ARG TARGETOS
ARG TARGETARCH
COPY .node-version dockerbuild/setup.sh /
RUN /setup.sh
