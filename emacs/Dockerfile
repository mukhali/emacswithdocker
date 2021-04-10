FROM ubuntu:20.04
ARG DEBIAN_FRONTEND=noninteractive

ARG ONAME=focal

RUN echo "\
deb http://mirrors.ustc.edu.cn/ubuntu/ ${ONAME} main restricted universe multiverse\n\
deb http://mirrors.ustc.edu.cn/ubuntu/ ${ONAME}-security main restricted universe multiverse\n\
deb http://mirrors.ustc.edu.cn/ubuntu/ ${ONAME}-updates main restricted universe multiverse\n\
deb http://mirrors.ustc.edu.cn/ubuntu/ ${ONAME}-backports main restricted universe multiverse\n\
" > /etc/apt/sources.list;

RUN apt-get update && \
    apt-get install -y \
            git \
            curl \
            gnupg \
            emacs \
            wget \
    && rm -rf /var/lib/apt/lists/*

ARG UNAME=docker
RUN useradd --create-home --shell /bin/bash ${UNAME} && \
    echo "${UNAME}:${UNAME}" | chpasswd && \
    adduser ${UNAME} sudo && \
    chown -R ${UNAME}: /home/${UNAME}

RUN groupdel video &> /dev/null || true && \
    groupadd -fog 44 video && \
    gpasswd -a "${UNAME}" video && \
    groupdel dialout &> /dev/null || true  && \
    groupadd -fog 20 dialout || true   && \
    gpasswd -a "${UNAME}" dialout

USER ${UNAME}

CMD ["/bin/sh", "-c", "trap 'exit 147' TERM; tail -f /dev/null & wait ${!}"]
