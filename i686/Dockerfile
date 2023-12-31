FROM ubuntu:latest

ENV DEBIAN_FRONTEND=noninteractive 
ENV TARGET=i686
# Build Enviroment
ENV BUILD_TARGET=i686-elf
ENV BUILD_PREFIX=/usr/local
ENV PATH="$BUILD_PREFIX/bin:$PATH"
# Package Versions
ENV BINUTILS_PKG_NAME=binutils-2.41
ENV GCC_PKG_NAME=gcc-13.2.0

# Update and install dependency
RUN apt-get -qy update
RUN apt-get -qy install \
        build-essential \
        bison           \
        flex            \
        libgmp3-dev     \
        libmpc-dev      \
        libmpfr-dev     \
        texinfo         \
        make            \
        wget            \
        libmpc-dev

# Binutils Build
RUN wget -q http://ftp.gnu.org/gnu/binutils/$BINUTILS_PKG_NAME.tar.gz                   && \
    tar xzf $BINUTILS_PKG_NAME.tar.gz                                                   && \
    mkdir -p /build/binutils/                                                           && \
    cd /build/binutils/                                                                 && \
    /$BINUTILS_PKG_NAME/configure --target="$BUILD_TARGET" --prefix="$BUILD_PREFIX"        \
    --with-sysroot --disable-nls --disable-werror                                       && \
    make -j$(nproc)                                                                     && \
    make install                                                                        && \
    rm -rf /$BINUTILS_PKG_NAME* /build/

# GCC Build
RUN wget -q https://ftp.gnu.org/gnu/gcc/$GCC_PKG_NAME/$GCC_PKG_NAME.tar.gz              && \
    tar xzf $GCC_PKG_NAME.tar.gz                                                        && \
    cd /$GCC_PKG_NAME && contrib/download_prerequisites                                 && \
    mkdir -p /build/gcc/                                                                && \
    cd /build/gcc/                                                                      && \
    /$GCC_PKG_NAME/configure --target="$BUILD_TARGET" --prefix="$BUILD_PREFIX"             \
    --disable-nls --enable-languages=c,c++ --without-headers                            && \
    make -j$(nproc) all-gcc                                                             && \
    make -j$(nproc) all-target-libgcc                                                   && \
    make install-gcc                                                                    && \
    make install-target-libgcc                                                          && \
    rm -rf /$GCC_PKG_NAME* /build/

# Clean
RUN apt-get -qy autoremove binutils gcc g++ bison flex libgmp3-dev             \
    libmpc-dev libmpfr-dev texinfo                                          && \
    apt-get clean                                                           && \
    apt-get autoremove -y                                                   && \
    rm -rf /var/lib/apt /var/lib/dpkg /var/lib/cache /var/lib/log

# Workdir
WORKDIR /env

