FROM buildpack-deps:jessie
LABEL maintainer="Peter Martini <PeterCMartini@GMail.com>, Zak B. Elep <zakame@cpan.org>"

WORKDIR /usr/src/perl

RUN true \
    && curl -SL https://www.cpan.org/src/5.0/perl-5.30.0-RC2.tar.xz -o perl-5.30.0-RC2.tar.xz \
    && echo 'fcff23e2e8349cd001af32adbbda3e80ccac32c9892cbb8687e181da412cd7bc *perl-5.30.0-RC2.tar.xz' | sha256sum -c - \
    && tar --strip-components=1 -xaf perl-5.30.0-RC2.tar.xz -C /usr/src/perl \
    && rm perl-5.30.0-RC2.tar.xz \
    && gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
    && archBits="$(dpkg-architecture --query DEB_BUILD_ARCH_BITS)" \
    && archFlag="$([ "$archBits" = '64' ] && echo '-Duse64bitall' || echo '-Duse64bitint')" \
    && ./Configure -Darchname="$gnuArch" "$archFlag" -Duseshrplib -Dvendorprefix=/usr/local  -des \
    && make -j$(nproc) \
    && TEST_JOBS=$(nproc) make test_harness \
    && make install \
    && cd /usr/src \
    && curl -LO https://www.cpan.org/authors/id/M/MI/MIYAGAWA/App-cpanminus-1.7044.tar.gz \
    && echo '9b60767fe40752ef7a9d3f13f19060a63389a5c23acc3e9827e19b75500f81f3 *App-cpanminus-1.7044.tar.gz' | sha256sum -c - \
    && tar -xzf App-cpanminus-1.7044.tar.gz && cd App-cpanminus-1.7044 && perl bin/cpanm . && cd /root \
    && true \
    && rm -fr ./cpanm /root/.cpanm /usr/src/perl /usr/src/App-cpanminus-1.7044* /tmp/*

WORKDIR /root

CMD ["perl5.28.2","-de0"]
