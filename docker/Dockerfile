FROM node:12

ARG USERNAME=vscode
ARG NPM_GLOBAL=/usr/local/share/npm-global

EXPOSE 3000

# Configure apt and install packages
RUN apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends apt-utils dialog 2>&1 \
    #
    # install git iproute2, procps, lsb-release (useful for CLI installs)
    && apt-get -y install git openssh-client less iproute2 procps iproute2 lsb-release \
    #
    # Alter node user as needed 
    && if [ "$USERNAME" != "node" ]; then \
    groupmod -n $USERNAME node \
    && usermod -l $USERNAME node; \
    fi \
    #
    # [Optional] Add sudo support for the non-root user
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME\
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    #
    # Set alternate global install location that both users have rights to access
    && mkdir -p ${NPM_GLOBAL} \
    && chown ${USERNAME}:root ${NPM_GLOBAL} \
    && npm config -g set prefix ${NPM_GLOBAL} \
    && sudo -u ${USERNAME} npm config -g set prefix ${NPM_GLOBAL} \    
    #    
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*
