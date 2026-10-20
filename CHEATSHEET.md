# herdr cheatsheet

herdr 0.8.0 · config: `~/.config/herdr/config.toml` · full manual:
https://claude.ai/code/artifact/993687a3-2e5c-45c6-b651-226722c1c2e8

Prefix is `ctrl+b` (tmux stays on `C-Space` — they don't collide).

## Session

    herdr                     launch or attach
    ctrl+b q                  detach (everything keeps running)
    herdr status              client + server status
    herdr config check        validate config.toml
    ctrl+b shift+R            reload config live

## Panes

    ctrl+b v                  split right
    ctrl+b -                  split down
    ctrl+h j k l              focus left / down / up / right  (no prefix)
    ctrl+b h j k l            same, via prefix (still works)
    ctrl+b z                  zoom focused pane
    ctrl+b r                  resize mode
    ctrl+b x                  close pane
    ctrl+b e                  open scrollback in nvim

## Workspaces & tabs

    ctrl+b shift+N            new workspace (prompts for name)
    ctrl+b w                  workspace picker
    ctrl+b g                  navigate mode
    ctrl+b c                  new tab (no prompt)
    ctrl+b 1..9               switch tab
    ctrl+b n / p              next / previous tab
    ctrl+b shift+G            new git worktree workspace
    ctrl+b b                  toggle sidebar

## Custom (added for you)

    ctrl+b shift+L            lazygit popup   (replaces `lg`)
    ctrl+b shift+Y            yazi popup      (replaces `y`)

## Agent control (from inside a herdr pane)

    herdr pane split --current --direction right --cwd "$PWD" --no-focus
    herdr agent start reviewer --kind claude --pane <pane-id>
    herdr agent prompt reviewer "Review the current diff." --wait --timeout 120000
    herdr agent read reviewer --source recent-unwrapped --lines 120
    herdr agent list

Read pane IDs out of the JSON responses; don't guess them.

## Remote

    herdr --remote waterhub@waterhub-api.com
    herdr --remote wis@172.16.10.58 --session api

Requires herdr installed on the remote host. Plain `ssh` in a pane still
works fine for quick checks.

## Suggested layouts

Go API (api-wis, api-bca, waterhub-be, edge-api):

    pane 1  air
    pane 2  make migration-up / make swag / make swag-format
    pane 3  make docker-local / docker compose logs -f

Bun frontend (bbp-web, cms-wis, woori_web):

    pane 1  vim .
    pane 2  bun dev

## Gotchas

- `ctrl+h/j/k/l` is herdr's now. herdr is the outer layer, so those chords
  never reach the pane's program: vim-tmux-navigator is dead inside herdr
  panes (local nvim splits and tmux on remote hosts alike). Use the tmux
  prefix `C-Space` + `hjkl` for the inner layer. Shell readline loses
  `ctrl+l` (clear), `ctrl+k` (kill-line), `ctrl+h` (backspace),
  `ctrl+j` (accept-line) — `clear` and Enter still work.
- SVN checkouts show a blank `branch` cell in the sidebar — those tokens
  read git only. Expected.
- No macOS toasts? Set `ui.toast.delivery = "terminal"` or `"herdr"`.
- Don't launch herdr inside a herdr pane (`allow_nested` is off).
- After `herdr update`, trust `herdr --help` over this file.

## Undo

    herdr integration uninstall claude
    rm ~/.config/herdr/config.toml
    rm ~/.oh-my-zsh/custom/completions/_herdr && rm -f ~/.zcompdump*
    herdr config reset-keys        # keybindings only
