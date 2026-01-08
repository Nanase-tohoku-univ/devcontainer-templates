FROM paperist/texlive-ja:latest

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    sudo file xz-utils vim less librsvg2-bin \
    curl cmake make git wget \
    && rm -rf /var/lib/apt/lists/*

RUN wget -P "/sty" "https://raw.githubusercontent.com/h-kitagawa/plistings/master/plistings.sty"

RUN tlmgr update --self \
    && tlmgr install algorithms algorithmicx algpseudocodex

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000

RUN groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd -m -s /bin/bash --uid ${USER_UID} --gid ${USER_GID} ${USERNAME} \
    && echo "${USERNAME} ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

USER ${USERNAME}
