FROM python:3.10-alpine@sha256:0c46c7f15ee201a2e2dc3579dbc302f989a20b1283e67f884941e071372eb2cc

RUN set -eux; \
	apk add --no-cache --virtual .build-deps \
	cmake \
	ninja \
	pkgconfig \
	build-base \
	cairo-dev \
	fontconfig-dev \
	freetype-dev \
	gettext-dev \
	giflib-dev \
	glib-dev \
	libjpeg-turbo-dev \
	libpng-dev \
	tiff-dev \
	pango-dev \
	readline-dev \
	woff2-dev \
	libxml2-dev \
	xz; \
	apk add --no-cache cairo \
	fontconfig \
	freetype \
	gettext \
	giflib \
	glib \
	libjpeg-turbo \
	libpng \
	tiff \
	libtool \
	pango \
	readline \
	woff2 \
	libxml2 \
	&& tempDir="$(mktemp -d)"; cd "$tempDir"; \
	wget -q https://github.com/fontforge/libspiro/releases/download/20220722/libspiro-dist-20220722.tar.gz \
	&& echo '3b8e54473f3d4d99c014f2630e62f966f5f4e25c28ca59b63d30bd8e9b7593f5  libspiro-dist-20220722.tar.gz' | sha256sum -c \
	&& tar xzf libspiro-dist-20220722.tar.gz \
	&& cd libspiro-20220722/ \
	&& ./configure \
	&& make install; \
	cd "$tempDir"; wget -q https://github.com/fontforge/fontforge/releases/download/20220308/fontforge-20220308.tar.xz \
	&& echo '01e4017f7a0ccecf436c74b8e1f6b374fc04a5283c1d68967996782e15618e59  fontforge-20220308.tar.xz' | sha256sum -c \
	&& tar xf fontforge-20220308.tar.xz \
	&& cd fontforge-20220308/ \
	&& cmake -S . -B build -DCMAKE_BUILD_TYPE=Release -DCMAKE_VERBOSE_MAKEFILE=ON -Wno-dev -DBUILD_TESTING=OFF -GNinja -DENABLE_GUI=OFF -DENABLE_FONTFORGE_EXTRAS=ON -DPython3_EXECUTABLE=/usr/local/bin/python3 \
	&& cmake --build build \
	&& cmake --install build; \
	cd /; rm -rf "$tempDir"; \
	apk del --no-cache --no-network .build-deps;
