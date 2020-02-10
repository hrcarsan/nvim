FROM alpine:3.8

RUN apk update --no-cache \
 ## install runtime dependencies =========================
 && apk add --no-cache \
    jansson yaml libxml2 php xclip git nodejs npm  python3 py-pip \
    bash gperf lua5.1-lpeg lua5.1-mpack gettext-dev libtermkey-dev libuv-dev libvterm-dev \
    luajit-dev msgpack-c-dev unibilium-dev findutils perl-dev diffutils \
 ## install build dependencies ==========================
 && apk add --no-cache --virtual .build-deps \
      cmake make g++ curl python-dev python3-dev \
      autoconf gcc automake musl-dev jansson-dev yaml-dev libxml2-dev \
 ## install nvim =======================================
 && cd /tmp/ \
 && export NVIM_VERSION=0.3.8 \
 && wget https://github.com/neovim/neovim/archive/v$NVIM_VERSION.tar.gz \
 && tar -xzvf v$NVIM_VERSION.tar.gz \
 && cd neovim-$NVIM_VERSION/ \
 && mkdir build \
 && cd build/ \
 && cmake .. -DCMAKE_BUILD_TYPE=MinSizeRel -DCMAKE_INSTALL_PREFIX=/usr \
             -DCMAKE_INSTALL_LIBDIR=lib -DENABLE_JEMALLOC=FALSE -DCMAKE_VERBOSE_MAKEFILE=TRUE \
             -DCMAKE_BUILD_TYPE=RelWithDebInfo \
 && make \
 && make install \
 && pip2 install --upgrade pip \
 && pip3 install --upgrade pip \
 && pip2 install --upgrade neovim \
 && pip3 install --upgrade neovim \
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
 ## clean ===========================================
 && runDeps="$( \
		scanelf --needed --nobanner --recursive /usr/local \
			| awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
			| sort -u \
			| xargs -r apk info --installed \
			| sort -u \
	)" \
 && apk add --no-cache --virtual .rundeps $runDeps \
 && rm -rf /tmp/* && apk del --no-cache .build-deps

WORKDIR /workspace
