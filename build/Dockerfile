# Alpine Linux glibc with calibre
# frolvlad/alpine-glibc based

FROM youske/alpine-conda
MAINTAINER youske miyakoshi <youske@gmail.com>

LABEL com.geeplus.description="alpine calibre image" \
      com.geeplus.role="baseimage"

ENV CALIBRE_LIBRARY=/calibre_library \
    CALIBRE_URL=https://raw.githubusercontent.com/kovidgoyal/calibre/master/setup/linux-installer.py

ENV PACKAGE='bash ca-certificates libstdc++ openssh-client poppler mesa-gl xdg-utils imagemagick' \
    ADDPACKAGE='dateutil pil pyqt pygments' \
    BUILD_PACKAGE='wget git build-base linux-headers' \
    LANG=C.UTF-8 \
    PORT=8080 \
    USER=admin \
    PASSWORD=admin \
    MAX_COVER=300x400 \
#    PATH=${CALIBRE_BASE}/bin:$PATH \
    WORK_DIR=/

# apk install
RUN apk update --no-cache && apk add --no-cache ${PACKAGE} && apk add --no-cache ${BUILD_PACKAGE} -t buildpack && \
    conda update -y --all && conda install -y ${ADDPACKAGE}

# calibre install
RUN wget -q --no-check-certificate ${CALIBRE_URL} -O- | python -c "import sys; main=lambda:sys.stderr.write('Download Failed\n'); exec(sys.stdin.read()); main()"

# calibre setup
RUN mkdir ${CALIBRE_LIBRARY} && mkdir ${CALIBRE_LIBRARY}/import

# remove buildoack
RUN apk del buildpack --no-cache && rm -rf /tmp/* /var/cache/apk/*

EXPOSE ${PORT}

WORKDIR ${WORK_DIR}
ENTRYPOINT ["switch","shell=/bin/bash","--","calibre-server","--auto-reload","--with-library","${CALIBRE_LIBRARY}","--username","${USER}","--password","${PASSWORD}","--max-cover","${MAX_COVER}","--port","${PORT}"]
