Update dependencies
```bash
nix-channel --update
nix-env -iA nixpkgs.nix
nix flake update
```

Work with Dotfiles using chezmoi
```bash
chezmoi re-add
chezmoi apply
```

Rebuild Nix
```bash
darwin-rebuild switch --flake ~/.config/nix-darwin
```