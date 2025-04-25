FROM ubuntu:jammy

# Set non-interactive mode for apt
ENV DEBIAN_FRONTEND=noninteractive
ENV TERM=xterm-256color

# Install dependencies

RUN apt-get update && apt-get install -y software-properties-common && \
    apt-add-repository ppa:git-core/ppa -y && \
    apt-add-repository ppa:neovim-ppa/unstable -y


RUN apt-get update && apt-get install -y \
    curl \
    wget \
    git \
    zsh \
    neovim \
    tmux \
    sudo \
    gnupg2 \
    ca-certificates \
    unzip \
    ripgrep \
    fd-find \
    build-essential \
    command-not-found \
    && apt-get clean && rm -rf /var/lib/apt/lists/*


RUN echo 'deb http://download.opensuse.org/repositories/home:/justkidding/xUbuntu_22.04/ /' | sudo tee /etc/apt/sources.list.d/home:justkidding.list
RUN curl -fsSL https://download.opensuse.org/repositories/home:justkidding/xUbuntu_22.04/Release.key | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/home_justkidding.gpg > /dev/null

RUN apt-get update && apt-get install -y ueberzugpp

# Optional: Create a non-root user
ARG USERNAME=svic22
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Install fzf
# RUN git clone --depth 1 https://github.com/junegunn/fzf.git /home/$USERNAME/.fzf && \
#     /home/$USERNAME/.fzf/install --all --no-bash --no-fish

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME -s /bin/zsh \
    && echo "$USERNAME ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# Set user
USER $USERNAME
WORKDIR /home/$USERNAME
# Optional: install oh-my-zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended

# fzf install
# RUN git clone --depth 1 https://github.com/junegunn/fzf.git /home/$USERNAME/.fzf && \
#     /home/$USERNAME/.fzf/install --all --no-bash --no-fish

# Install Powerlevel10k theme
# RUN git clone --depth=1 https://github.com/romkatv/powerlevel10k.git /home/$USERNAME/.oh-my-zsh/custom/themes/powerlevel10k \
#     && sed -i 's/ZSH_THEME=".*"/ZSH_THEME="powerlevel10k\/powerlevel10k"/' /home/$USERNAME/.zshrc
# Set Zsh as default shell
SHELL ["/bin/zsh", "-c"]

# RUN if git clone --bare git@github.com:svic321/dotfiles.git $HOME/.dotfiles; then \
#         echo "Cloned using SSH"; \
#         touch withssh.txt; \
#     else \
#         echo "Falling back to HTTPS"; \
#         git clone --bare https://github.com/svic321/dotfiles.git $HOME/.dotfiles; \
#         touch withhttps.txt; \
#     fi

RUN wget https://raw.githubusercontent.com/svic321/dotfiles/refs/heads/master/.svic321/bin/install
# WORKDIR /home/.svic321

# RUN ./bin/install

# Optional: Install plugin managers or other CLI tools here
# Example: Install vim-plug for Neovim
# RUN curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
#      https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

# Default command
CMD ["zsh"]
