## Main apps
Install [`yay`](https://github.com/Jguer/yay) (package manager for AUR)
Packages from official repository
```bash
sudo pacman -S firefox konsole dolphin spotify-launcher telegram-desktop obsidian code fish keepass ttf-meslo-nerd fastfetch
```
Packages from AUR
```bash
yay -S vesktop exodus code-features
```
## Setup terminal
### Enhance fish
Install [`fisher`](https://github.com/jorgebucaran/fisher  ) (Fish plugin manager)
Install `fisher` plugins
```bash
fisher install IlanCosman/tide@v6 jorgebucaran/nvm.fish
```
### Git credential manager
AUR package didn't work for me so I recommend to install from original [source](https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/install.md)
```bash
curl -L https://aka.ms/gcm/linux-install-source.sh | sh
git-credential-manager configure
```