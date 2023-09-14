FROM centos:7

RUN yum install -y \
    'pkgconfig(libselinux)' \
    /usr/bin/eu-readelf \
    autoconf \
    automake \
    docbook-style-xsl \
    gcc \
    git \
    libasan \
    libcap-devel \
    libtool \
    libxslt \
    make \
    rpm-build \
    rpmdevtools

RUN git clone https://github.com/dmikushin/bubblewrap.git && \
    cd bubblewrap && \
    mkdir -p /root/rpmbuild/SOURCES && \
    spectool -g -R packaging/bubblewrap.spec && \
    rpmbuild -bb packaging/bubblewrap.spec

