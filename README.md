# cli-sync

Keeps the WSL cli setup matched to Windows so nothing has to be installed or configured twice. Windows is the source of truth, WSL just points at it.

## Usage

```bash
ln -s /mnt/c/Users/JonahW/Projects/cli-sync/clisync ~/.local/bin/clisync

clisync          # link configs, pin tools, restore nvim plugins
clisync update   # scoop update * on windows, then sync
clisync status   # what's linked and which tool versions differ
```

From Windows: `wsl -- clisync update`

## What it does

- symlinks nvim, yazi, oh-my-posh, lazygit and fastfetch configs to the Windows ones
- reads the oh-my-posh theme from the pwsh profile and puts the same one in `~/.bashrc` (between `# >>> clisync` markers)
- `~/.gitconfig` includes the Windows one, sets `autocrlf = input` so Windows checkouts don't all show as modified, and uses the Windows credential manager
- copies `~/.ssh` over (copied not linked, ssh refuses keys with drvfs permissions)
- links Claude Code settings, skills, agents, commands, and each project's memory folder. Reinstalls enabled plugins since their install paths are Windows paths
- pins every tool in the `TOOLS` table to the version scoop has, through `mise use -g`. If mise doesn't have that version yet it uses latest and warns
- `Lazy! restore` so nvim plugins match `lazy-lock.json`

Anything already in the way gets moved to `<name>.bak.<time>`.

To sync another scoop tool add it to `TOOLS`. Things WSL needs that aren't from scoop go in `EXTRA`.
