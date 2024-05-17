# Bun development environment
FROM oven/bun:debian

# Install dependencies, Neovim, tmux, and git
# This is a lot of dependencies, but I will have to make due on the dev image
RUN apt-get update && \
    apt-get install -y tmux git curl wget zsh npm cmake make unzip gcc jq openssl iputils-ping dnsutils ninja-build gettext pkg-config libtool autoconf g++ libtool-bin && \
        rm -rf /var/lib/apt/lists/*

# Install Neovim, use stable release of 0.9
RUN apt remove neovim -y
RUN git clone https://github.com/neovim/neovim
WORKDIR /neovim
RUN git checkout remotes/origin/release-0.9
RUN make CMAKE_BUILD_TYPE=RelWithDebInfo

# Create a Debian package
WORKDIR /neovim/build
RUN cpack -G DEB

# Install the Neovim package
RUN dpkg -i --force-overwrite nvim-linux64.deb

# Use environment variables to clone Neovim and tmux configurations
ENV NVIM_CONF_REPO="https://github.com/mk3-14159/dotfiles.git"

RUN git clone $NVIM_CONF_REPO /root/.config/nvim
RUN mv /root/.config/nvim/.tmux.conf /root/
RUN mv /root/.config/nvim/.zshrc /root/

# Change the default shell to zsh for the root user
SHELL ["/bin/zsh", "-c"]

# Install Oh My Zsh and setup .zshrc
RUN sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"

# Set the working directory to the home directory
WORKDIR /root

# Configure git with a default username and email
RUN git config --global user.name "mk.dev"
RUN git config --global user.email "mk@dev.com"

# Configure ssh default key 
COPY ~/.ssh/id_ed25519 /root/.ssh/id_ed25519
RUN chmod 600 /root/.ssh/id_ed25519
RUN ssh-keyscan github.com >> /root/.ssh/known_hosts

# Set the default command to open tmux which then opens Neovim
CMD ["zsh"]
