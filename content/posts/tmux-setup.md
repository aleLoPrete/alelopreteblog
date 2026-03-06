+++
title = "Tmux Setup"
date = 2023-10-19

[taxonomies]
tags = ["productivity"]
+++

![tmux 4 panes](/img/tmux/tmux-4pane.jpg)

## How I Use Tmux

Tmux is a terminal multiplexer: it lets you open multiple panes, windows, and sessions seamlessly. I started using it for a neatly organized terminal setup and am perfecting its use while working through CTF challenges. Being efficient in moving between terminal windows is essential for ease of mind. Additionally, combining tmux's pane splitting with `pwntools` scripts gives a competitive edge when debugging exploits.

## Prefix

```bash
# Set prefix to Ctrl-a
unbind C-b
set -g prefix C-a
```

The prefix is a key combination that initiates any tmux command. It must be comfortable and easily reachable.

When learning touch typing, you keep your hands on the home row. In QWERTY, the home row starts with `a`. This is also why I always swap Ctrl and Caps Lock on all my setups. Ctrl-a follows naturally from that choice.

## Pane Navigation and Window Splitting

```bash
# Vim-like bindings for pane navigation
unbind h
bind h select-pane -L
unbind j
bind j select-pane -D
unbind k
bind k select-pane -U
unbind l
bind l select-pane -R

# Intuitive window-splitting keys
bind ] split-window -h -c '#{pane_current_path}'
bind - split-window -v -c '#{pane_current_path}'
```

Once you learn Vim's core motions, nothing else makes sense.

```bash
# Escape time for NeoVim
set-option -sg escape-time 10
```

NeoVim and tmux shortcuts may conflict; this setting makes them work smoothly together.

## Minimal Status Bar

```bash
# Plugin
set -g @plugin 'niksingh710/minimal-tmux-status'

# Initialize TMUX plugin manager (keep at the bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```

One plugin for a minimal status bar.

## Daily Setup

My standard layout:

- **Window 1:** Neovim + zsh
- **Window 2:** zsh + gdb

![tmux setup 1](/img/tmux/tmux-setup1.png)

![tmux setup 2](/img/tmux/tmux-setup2.png)

Window management:

```bash
<Prefix> + n    # next window
<Prefix> + c    # create window
<Prefix> + x    # kill window
<Prefix> + ,    # rename window
```
