```bash
sudo apt update
sudo apt upgrade
```

https://github.com/mintty/wsltty
https://github.com/ohmyzsh/ohmyzsh

```bash
sudo apt install neovim

gcl git@github.com:2jacobtan/dotfiles.git

rm -r $ZSH_CUSTOM
ln -sv ~/dotfiles/zsh_custom $ZSH_CUSTOM

cp ~/dotfiles/.zshrc ~/.zshrc

nvim ~/.zshrc # edit as required

exec zsh
```
