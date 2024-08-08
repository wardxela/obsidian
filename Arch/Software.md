## Main apps
Install [`yay`](https://github.com/Jguer/yay) (package manager for AUR)
Packages from official repository
```sh
sudo pacman -S firefox konsole dolphin spotify-launcher telegram-desktop obsidian code fish keepass ttf-meslo-nerd fastfetch
```
Packages from AUR
```sh
yay -S vesktop exodus code-features
```
## Setup terminal
### Enhance fish
Install [`fisher`](https://github.com/jorgebucaran/fisher  ) (Fish plugin manager)
Install `fisher` plugins
```sh
fisher install IlanCosman/tide@v6 jorgebucaran/nvm.fish
```
Set up default version for `NVM`:
```sh
set --universal nvm_default_version v18.4.0
```
### Git credential manager
AUR package didn't work for me so I recommend to install from original [source](https://github.com/git-ecosystem/git-credential-manager/blob/main/docs/install.md)
```sh
curl -L https://aka.ms/gcm/linux-install-source.sh | sh
git-credential-manager configure
git config --global credential.credentialStore plaintext
```