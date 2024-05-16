# Use an official base image with enough tools, e.g., Ubuntu
FROM ubuntu:latest

# Install dependencies, Neovim, tmux, and git
# This is a lot of dependencies, but I will have to make due on the dev image
RUN apt-get update && \
    apt-get install -y neovim tmux git curl wget zsh npm make unzip gcc jq && \
        rm -rf /var/lib/apt/lists/*

# Use environment variables to clone Neovim and tmux configurations
# ENV NVIM_CONF_REPO="https://github.com/mk3-14159/dotfiles.git"
ENV NVIM_CONF_REPO="https://github.com/mk3-14159/dotfiles.git"

RUN git clone $NVIM_CONF_REPO /root/.config/nvim
RUN mv /root/.config/nvim/.tmux.conf /root/
RUN mv /root/.config/nvim/.zshrc /root/

# Install Oh My Zsh and setup .zshrc
RUN sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"

# Change the default shell to zsh for the root user
SHELL ["/bin/zsh", "-c"]

# Set the working directory to the home directory
WORKDIR /root

# Set the default command to open tmux which then opens Neovim
#CMD ["tmux", "new-session", "nvim"]
CMD ["zsh"]
