FROM debian:10-slim
MAINTAINER Santiago Herrera Cardona <santiagohecar@gmail.com>

RUN set -ex; \
 apt-get update \
 ## install dependencies =============================
 && apt install -y ninja-build gettext libtool libtool-bin autoconf automake \
                   cmake g++ pkg-config unzip wget python3-dev python-dev python-pip python3-pip \
                   php xclip git \
 ## set locales  =====================================
 && DEBIAN_FRONTEND=noninteractive apt install -y locales \
 && sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8 \
 ## install nvim =====================================
 && export NVIM_VERSION=0.4.3 \
 && wget https://github.com/neovim/neovim/archive/v$NVIM_VERSION.tar.gz \
 && tar -xzvf v$NVIM_VERSION.tar.gz \
 && cd neovim-$NVIM_VERSION/ \
 && make CMAKE_BUILD_TYPE=Release \
 && make install \
 && pip2 install --upgrade pip \
 && pip3 install --upgrade pip \
 && pip2 install --upgrade neovim \
 && pip3 install --upgrade neovim \
 && pip3 install --user pynvim \
 && pip3 install --upgrade msgpack \
 ## install node.js ==================================
 && cd /tmp \
 && VERSION=v13.12.0 \
 && DISTRO=linux-x64 \
 && mkdir -p /usr/local/lib/nodejs \
 && wget http://nodejs.org/dist/$VERSION/node-$VERSION-$DISTRO.tar.gz \
 && tar -zxvf node-$VERSION-$DISTRO.tar.gz -C /usr/local/lib/nodejs \
 && ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/node /usr/bin/node \
 && ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/npm /usr/bin/npm \
 && ln -s /usr/local/lib/nodejs/node-$VERSION-$DISTRO/bin/npx /usr/bin/npx \
 && npm install -g neovim \
 ## install ripgrep ==================================
 && cd /tmp \
 && export RG_VERSION=0.8.1-x86_64-unknown-linux-musl \
 && wget https://github.com/BurntSushi/ripgrep/releases/download/0.8.1/ripgrep-$RG_VERSION.tar.gz \
 && tar -xzvf ripgrep-$RG_VERSION.tar.gz \
 && cd ripgrep-$RG_VERSION/ \
 && mv rg /usr/bin/ \
 ## install universal-ctags ==========================
 && cd /tmp \
 && git clone https://github.com/universal-ctags/ctags.git \
 && cd ctags \
 && ./autogen.sh \
 && ./configure \
 && make \
 && make install \
 # clean
 && rm -rf /tmp/* \
 && rm -r /var/lib/apt/lists/*;

ENV LANG en_US.UTF-8

WORKDIR /workspace
