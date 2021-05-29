```bash
sudo apt update
sudo apt upgrade
```

https://github.com/mintty/wsltty
add a '~' to the end of wsltty's shortcut, so that it opens Ubuntu in home folder
![[Pasted image 20210530024443.png]]

```bash
apt install zsh
```

https://github.com/ohmyzsh/ohmyzsh

https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

add SSH public key to github
https://github.com/settings/keys

```bash
sudo apt install neovim

gcl git@github.com:2jacobtan/dotfiles.git

rm -r $ZSH_CUSTOM
ln -sv ~/dotfiles/zsh_custom $ZSH_CUSTOM

cp ~/dotfiles/.zshrc ~/.zshrc

nvim ~/.zshrc # edit as required

exec zsh
```
