# Josh set up guide: Mac mini with runner-up + firstmate

Do these in order. Each step is one thing. Total time: about 30 minutes.

## 1. Set up the Mac

1. Finish macOS Setup Assistant (create your user, connect to Wi-Fi).
2. Open **System Settings > General > Sharing** and turn on **Remote Login** (SSH).
3. Open **Terminal** (press Cmd+Space, type `Terminal`, press Enter).

## 2. Install the basics

1. Install Homebrew: `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
2. Follow the two "Next steps" lines Homebrew prints (they add it to your PATH).
3. Install the tools: `brew install gh git node herdr && brew install --cask wezterm tailscale`
4. Open Tailscale from Applications and sign in.
5. Sign in to GitHub: `gh auth login` (choose GitHub.com, HTTPS, browser).
6. Open **WezTerm** and do the rest of this guide in it.

## 3. Fork and install runner-up

1. Fork it: `gh repo fork kunchenguid/runner-up --clone=false`
2. Install it: `curl -fsSL https://kunchenguid.github.io/runner-up/install.sh | sh`
3. Put `~/.local/bin` on your PATH: `echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc`
4. Check it runs: `runner-up --help`

## 4. Point runner-up at your repo and this Mac

1. Replace `YOU/REPO` with the repo whose jobs should run here: `runner-up init --repo YOU/REPO --pool my-mac`
2. Add this Mac (replace `YOU` with your Mac username): `runner-up machine add mac --ssh YOU@localhost --provider host`
3. Check the setup: `runner-up check-config`
4. Start it: `runner-up serve`
5. In a second Terminal tab, confirm: `runner-up status`

Use it in a workflow with `runs-on: my-mac`.

## 5. Install firstmate

1. `git clone https://github.com/kunchenguid/firstmate`
2. `cd firstmate`
3. Install the firstmate helper tools (`gh-axi`, `chrome-devtools-axi`, `tasks-axi`, `treehouse`, `no-mistakes`): follow the install section of the firstmate README in this folder. If a command is missing, `claude` will tell you which one.
4. `claude` (if the command is missing: `brew install --cask claude-code`)

## 6. Install herdr web

herdr web is a browser view of your herdr panes.

1. Start herdr in WezTerm: `herdr`
2. Download the macOS tarball from https://github.com/kcosr/herdr-web/releases (latest release).
3. Unpack it and run: `tar -xzf herdr-web-*.tar.gz && cd herdr-web-* && bin/herdr-web`
4. Open http://127.0.0.1:8787 in your browser. Over Tailscale, use `http://YOUR-MAC-NAME:8787` from your phone.

Done: your Mac runs your GitHub jobs, and herdr, herdr web, WezTerm, Tailscale and firstmate are all installed.

## If something fails

- `runner-up status` shows no places: rerun step 4.2.
- `ssh YOU@localhost true` asks for a password: confirm Remote Login is on (step 1.2) and run `ssh-copy-id YOU@localhost`.
- Anything else: send the exact error text to Trillium.
