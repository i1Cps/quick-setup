## Basic build tools

```
sudo apt update
sudo apt install curl git build-essential
```

## Install Brave

```
sudo apt install curl

sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg] https://brave-browser-apt-release.s3.brave.com/ stable main"|sudo tee /etc/apt/sources.list.d/brave-browser-release.list

sudo apt update

sudo apt install brave-browser
```

## Install GIT

```
sudo apt install git
```

## Install Nvim

```
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim-linux-x86_64.appimage
chmod u+x nvim-linux-x86_64.appimage
./nvim-linux-x86_64.appimage

sudo mkdir -p /opt/nvim
sudo mv nvim-linux-x86_64.appimage /opt/nvim/nvim

echo 'export PATH="$PATH:/opt/nvim/"' >> ~/.bashrc
source ~/.bashrc

./nvim-linux-x86_64.appimage --appimage-extract
./squashfs-root/AppRun --version

# Optional: exposing nvim globally.
sudo mv squashfs-root /
sudo ln -s /squashfs-root/AppRun /usr/bin/nvim
nvim
```

## Install Lazyvim

```
# required
mv ~/.config/nvim{,.bak}

# optional but recommended
mv ~/.local/share/nvim{,.bak}
mv ~/.local/state/nvim{,.bak}
mv ~/.cache/nvim{,.bak}

git clone https://github.com/LazyVim/starter ~/.config/nvim

rm -rf ~/.config/nvim/.git
```

## Config Lazyvim quickly

```
sed -i '1ivim.opt.clipboard = "unnamedplus"\nrequire("config.autocmds")\n' ~/.config/nvim/lua/config/lazy.lua

cat << 'EOF' > ~/.config/nvim/lua/config/autocmds.lua
vim.api.nvim_create_autocmd("FileType", {
  pattern = "python",
  callback = function()
    vim.b.autoformat = false
  end,
})
EOF

# Make sure the plugins/ folder exists
mkdir -p ~/.config/nvim/lua/plugins

# Create the override for folke/tokyonight.nvim
cat << 'EOF' > ~/.config/nvim/lua/plugins/tokyonight.lua
return {
  {
    "folke/tokyonight.nvim",
    -- load immediately and at a high priority so it overrides LazyVim’s default
    lazy     = false,
    priority = 1000,
    opts = {
      transparent = true,
      styles = {
        sidebars = "transparent",
        floats   = "transparent",
      },
    },
    config = function(_, opts)
      require("tokyonight").setup(opts)
      vim.cmd([[colorscheme tokyonight]])
    end,
  },
}
EOF

sudo apt install xclip
```

## Add Nerd fonts

```
mkdir -p ~/.local/share/fonts
wget -q https://github.com/ryanoasis/nerd-fonts/raw/master/patched-fonts/Hack/Regular/HackNerdFont-Regular.ttf -O ~/.local/share/fonts/HackNerdFont-Regular.ttf
fc-cache -fv
```

### 🖋 Set Terminal Font (Manual Step)

After installing the font, configure your terminal to use it:

1. Open your terminal settings/preferences.
2. Change the font to **Hack Nerd Font**.
3. Restart the terminal.

## Install tmux

```
sudo apt install tmux

mkdir -p ~/.config/tmux
curl -fsSL https://raw.githubusercontent.com/i1Cps/quick-setup/main/tmux/tmux.conf -o ~/.config/tmux/tmux.conf

git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm

tmux new-session -d
~/.tmux/plugins/tpm/bin/install_plugins
tmux kill-server

```

## Sort ssh key etc

```
ssh-keygen -t ed25519 -C "nohacks2701@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
```

Paste into github.com keys section

## Install exa

```
sudo apt install eza
cat << 'EOF' >> ~/.bashrc; source ~/.bashrc
# === eza aliases ===
alias ls='eza --icons'
alias ll='eza -l -g --git --icons'
alias lla='eza -la -g --git --icons'
EOF
```
