FROM python:slim

ARG USERNAME=dev
ARG UID=1000
ARG GID=1000

# Create the user
RUN groupadd --gid $GID $USERNAME \
    && useradd --uid $UID --gid $GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# install packages
RUN apt-get install -y \
    locales \
    mc \
    tree \
    git \
    make \
    wget

# cleanup
RUN rm -rf /var/lib/apt/lists/*

# set locale
RUN export LC_ALL=en_US.UTF-8
RUN export LANG=en_US.UTF-8
RUN locale-gen en_US.UTF-8

RUN pip install --upgrade pip

# cleanup
RUN rm -rf /var/lib/apt/lists/*

# install python packages
COPY requirements.txt /requirements.txt
RUN pip3 install -r /requirements.txt && rm /requirements.txt

# setup folders for saving vscode extensions
# https://code.visualstudio.com/remote/advancedcontainers/avoid-extension-reinstalls
RUN mkdir -p /home/$USERNAME/.vscode-server/extensions \
    && chown -R $USERNAME \
    /home/$USERNAME/.vscode-server


USER ${USERNAME}
RUN echo 'export PS1="🐳  \[\033[1;36m\]'"mkdocs"' \[\e[33m\]\W\[\e[m\] \[\033[1;36m\]# \[\033[0m\]"' >> ~/.bashrc



