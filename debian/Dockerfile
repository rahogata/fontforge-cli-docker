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
        xz-utils" \
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
        libxml2 \
        woff2 \
        wget" \
    ;\
    apt-get update; \
    printf "$buildDeps" | tr ' ' '\n' | xargs apt-get install -y --no-install-recommends \
    && printf "$runtimeDeps" | tr ' ' '\n' | xargs apt-get install -y --no-install-recommends \
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
        -DPython3_EXECUTABLE=/usr/local/bin/python3 \
    && cmake --build build \
    && cmake --install build \
    && cd /; rm -rf "$tempDir" \
    ;\
    printf "$buildDeps" | tr ' ' '\n' | xargs apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false ; \
    rm -rf /var/lib/apt/lists/*
