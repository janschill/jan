---
layout: post
title: "Terminal Bells with tmux, iTerm2, and Claude Code"
description: ""
date: 2026-03-28
tags: []
---
Being a 10x engineer these days requires ten parallel Claude Code sessions. To not neglect any of them, Anthropic recommends to [turn on notifications in your Terminal](https://code.claude.com/docs/en/terminal-config#notification-setup). This didn't really work for me because tmux, for some reason, swallowed the bell character `'\a'` and did not forward it to my outer Terminal, iTerm2, via `/dev/tty`. Additionally, I wanted a visual indicator in the tmux window when it wasn't focussed.

This is where I landed:

Inside iTerm2 under `Settings -> Profiles -> Terminal -> Bell`
![Screenshot 2026-03-28 at 14.41.30](/assets/images/2026/Screenshot 2026-03-28 at 14.41.30.png)

For tmux I added this to my configuration. Don't forget to source your new configuration file with
`tmux source-file ~/.conf/tmux/tmux.conf`

```conf
# ~/.config/tmux/tmux.conf
set -g allow-passthrough on
set -g bell-action any
set -g monitor-bell on
set -g visual-bell off
```

And lastly, a Claude hook pointing to a helper script

```json
  // ~/.claude/settings.json
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "bash ~/.claude/notify.sh"
          }
        ]
      }
    ],
    "PermissionRequest": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "bash ~/.claude/notify.sh"
          }
        ]
      }
    ]
  },

```

```sh
# ~/.claude/notify.sh

#!/bin/bash
# Skip if Claude's pane is already focused
if [ -n "$TMUX" ]; then
  ACTIVE=$(tmux display-message -p '#{pane_id}')
  [ "$ACTIVE" = "$TMUX_PANE" ] && exit 0
fi

# Bell for system alert sound
printf '\a' > /dev/tty

# Highlight Claude's tmux window tab in the status bar
if [ -n "$TMUX" ]; then
  TARGET=$(tmux display-message -p -t "$TMUX_PANE" '#{window_id}')

  tmux set -w -t "$TARGET" window-status-format "#[bg=#f5a623,fg=#1e1e2e,bold] #W #[default]"
  tmux set -w -t "$TARGET" window-status-current-format "#[bg=#f5a623,fg=#1e1e2e,bold] #W #[default]"

  (sleep 5; tmux set -wu -t "$TARGET" window-status-format; tmux set -wu -t "$TARGET" window-status-current-format) &
fi
```

Also, it will default to your *macOS Alert sound*. I recommend something other than the default, as that one can be a bit much to hear 100 times in a day. It should all feel like *a breeze*.

![Pasted image 20260328144835](/assets/images/2026/Pasted image 20260328144835.png)