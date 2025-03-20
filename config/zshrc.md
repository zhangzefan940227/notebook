if [[ -r "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh" ]]; then
  source "${XDG_CACHE_HOME:-$HOME/.cache}/p10k-instant-prompt-${(%):-%n}.zsh"
fi

# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH

# Path to your oh-my-zsh installation.
export ZSH="$HOME/.oh-my-zsh"

#ZSH_THEME="bira"
ZSH_THEME="powerlevel10k/powerlevel10k"
# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion.
# Case-sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

plugins=(git)

# add zsh-plugin
source ~/.oh-my-zsh/custom/plugins/fzf-tab/fzf-tab.plugin.zsh
source ~/.oh-my-zsh/custom/plugins/fzf-tab/fzf-tab.zsh
source ~/.oh-my-zsh/custom/plugins/zsh-fsh/fast-syntax-highlighting.plugin.zsh
source $ZSH/oh-my-zsh.sh

___MY_VMOPTIONS_SHELL_FILE="${HOME}/.jetbrains.vmoptions.sh"; if [ -f "${___MY_VMOPTIONS_SHELL_FILE}" ]; then . "${___MY_VMOPTIONS_SHELL_FILE}"; fi

[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh

export PATH=/opt/gradle/gradle-8.13/bin:$PATH
export PATH=~/Environment/kotlin-language-server/bin:$PATH
export PATH=/home/zzfan/.cargo/bin:$PATH
#THIS MUST BE AT THE END OF THE FILE FOR SDKMAN TO WORK!!!
export SDKMAN_DIR="$HOME/.sdkman"
[[ -s "$HOME/.sdkman/bin/sdkman-init.sh" ]] && source "$HOME/.sdkman/bin/sdkman-init.sh"
export LANG=en_US.UTF-8
alias vim='nvim'
alias vid='neovide ./ &'
alias lg='lazygit'
alias cl='clear'
alias fwk='~/Code/Android_Project/aosp/frameworks/'
alias aosp='~/Code/Android_Project/aosp/'
alias nb='~/Code/notebook/'
alias nvc='~/.config/nvim/'
alias zc='vim ~/.zshrc'
alias szc='source ~/.zshrc'
alias ls='lsd'
alias l='lsd -l'
alias ll='lsd -al'
alias fd='fdfind'
alias bo='bottom'
alias rr='ranger'

# To customize prompt, run `p10k configure` or edit ~/.p10k.zsh.
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh


plugins=( 
    # other plugins...
    zsh-autosuggestions
)

bindkey "\e[H" beginning-of-line
bindkey "\e[F" end-of-line
