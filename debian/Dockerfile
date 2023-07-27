FROM python:3.11-slim

RUN set -eux; \
# Build deps
    buildDeps="cmake \
        ninja-build \
        pkg-config \
        build-essential \
        libcairo2-dev \
        libfontconfig-dev \
        libfreetype-dev \
        libgif-dev \
        libglib2.0-dev \
        libjpeg-dev \
        libpng-dev \
        libspiro-dev \
        libtiff5-dev \
        libxml2-dev \
        libpango1.0-dev \
        libreadline-dev \
        libwoff-dev \
        xz-utils \
        wget \
        autoconf \
        automake" \
    ;\
# Runtime deps
    runtimeDeps="libcairo2 \
        fontconfig \
        libfreetype6 \
        gettext \
        libgif7 \
        libglib2.0-0 \
        libjpeg62-turbo \
        libpng16-16 \
        libspiro1 \
        libtiff6 \
        libtool \
        libpango1.0-0 \
        libreadline8 \
        libxml2 \
        woff2" \
    ;\
    apt-get update; \
    printf "$buildDeps" | tr ' ' '\n' | xargs apt-get install -y --no-install-recommends \
    && printf "$runtimeDeps" | tr ' ' '\n' | xargs apt-get install -y --no-install-recommends \
    && tempDir="$(mktemp -d)"; cd "$tempDir"; \
    wget https://github.com/fontforge/libuninameslist/releases/download/20221022/libuninameslist-dist-20221022.tar.gz \
    && echo '92c833936d653b2f205fb5e7ac82818311824dabdc7abdc2e81f07c3a0ea39bb  libuninameslist-dist-20221022.tar.gz' | sha256sum -c \
    && tar xf libuninameslist-dist-20221022.tar.gz \
    && cd libuninameslist-20221022/; \
       ./configure --prefix="/usr/local" \
    && make install \
    && cd /; rm -rf "$tempDir" \
    && tempDir="$(mktemp -d)"; cd "$tempDir"; \
    wget -q https://github.com/fontforge/fontforge/releases/download/20230101/fontforge-20230101.tar.xz \
    && echo 'ca82ec4c060c4dda70ace5478a41b5e7b95eb035fe1c4cf85c48f996d35c60f8  fontforge-20230101.tar.xz' | sha256sum -c \
    && tar xf fontforge-20230101.tar.xz \
    && cd fontforge-20230101/; \
    cmake -S . -B build \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_VERBOSE_MAKEFILE=ON \
        -Wno-dev \
        -DBUILD_TESTING=OFF \
        -GNinja \
        -DENABLE_GUI=OFF \
        -DENABLE_FONTFORGE_EXTRAS=ON \
        -DENABLE_DOCS=off \
        -DPython3_EXECUTABLE=/usr/local/bin/python3 \
        -DCMAKE_FIND_FRAMEWORK=LAST \
        -DENABLE_DOCS=off \
    && cmake --build build \
    && cmake --install build \
    && cd /; rm -rf "$tempDir" \
    ;\
    printf "$buildDeps" | tr ' ' '\n' | xargs apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false ; \
    rm -rf /var/lib/apt/lists/*
