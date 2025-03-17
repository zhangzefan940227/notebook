# 鼠标指针大小
gsettings set org.gnome.desktop.interface cursor-size 48

# fzf
git clone --depth 1 https://github.com/junegunn/fzf.git ./.fzf

# fzf-tab

git clone https://github.com/Aloxaf/fzf-tab ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/fzf-tab
# nvim-fzf
`git clone https://github.com/vijaymarupudi/nvim-fzf.git ~/.config/nvim/pack/plugins/start/nvim-fzf`

# powerlevel10k
`git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k`

# fsh
`git clone https://github.com/zdharma-continuum/fast-syntax-highlighting ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-fsh`

# cargo
sudo apt-get install cargo

# wudao
sudo apt-get install python3
sudo apt-get install python3-pip
sudo pip3 install bs4 --break-system-packages
sudo pip3 install lxml --break-system-packages

git clone https://github.com/chestnutheng/wudao-dict

# lazygit
[lazygit_0.48.0_Linux_32-bit.tar.gz](https://github.com/jesseduffield/lazygit/releases/download/v0.48.0/lazygit_0.48.0_Linux_32-bit.tar.gz)
[lazygit_0.48.0_Linux_x86_64.tar.gz](https://github.com/jesseduffield/lazygit/releases/download/v0.48.0/lazygit_0.48.0_Linux_x86_64.tar.gz)

# dust
`cargo install du-dust`

# bottom
`snap install bottom`

# lsd
`cargo install lsd`

# fd-find
`sudo apt install fd-find`

# ranger
sudo apt install ranger

# zsh-autosuggestions
```shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

Add the plugin to the list of plugins for Oh My Zsh to load (inside `~/.zshrc`):
plugins=( 
    # other plugins...
    zsh-autosuggestions
)
```