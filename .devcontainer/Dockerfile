FROM eclipse-temurin:21-jdk

# Create a non-root user
ARG USERNAME=vscode
ARG USER_UID=501
ARG USER_GID=$USER_UID

# Make sure the user has the correct permissions 
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

# Install basic development tools
RUN apt-get update && apt-get install -y \
    git \
    curl \
    wget \
    unzip \
    procps \
    less \
    vim \
    sudo \
    zsh \
    docker-cli

# Set the default shell to zsh for the non-root user
RUN chsh -s $(which zsh) $USERNAME

USER $USERNAME

# Pre-create vscode server directories with proper ownership for extensions cache and data for the non-root user
RUN mkdir -p /home/$USERNAME/.vscode-server/data \
    && mkdir -p /home/$USERNAME/.vscode-server/extensions 

# Pre-create Maven cache directory with proper ownership for the non-root user
# https://stackoverflow.com/questions/67020657/docker-can-not-write-on-mounted-volume-with-non-root-user#answer-67021906
RUN mkdir -p /home/$USERNAME/.m2 

# Pre-create Claude cache directory with proper ownership for the non-root user
RUN mkdir -p /home/$USERNAME/.claude

# Install Oh My Zsh and configure it for the non-root user
COPY --chown=$USERNAME:$USERNAME oh-my-zsh-config.sh /home/$USERNAME/.oh-my-zsh-config.sh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" \
    && echo "source /home/$USERNAME/.oh-my-zsh-config.sh" >> /home/$USERNAME/.zshrc
