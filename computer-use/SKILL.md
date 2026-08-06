---
name: computer-use
description: Use when a task requires visual interaction with macOS apps or websites, such as navigating interfaces, entering data, testing flows, or inspecting visual state.
---

# Computer Use

First, load the `/herdr` skill and follow it for all Herdr work, including its Herdr environment check.

Create a new Herdr tab in the current workspace without taking focus from the user. Keep the current working directory.

In the tab's first pane, use `herdr pane run` to run non-interactive Pi (`--print`) with GPT-5.6 Luna at xhigh reasoning:

```bash
pi --provider openai-codex --model gpt-5.6-luna --thinking xhigh --print
```

Give the agent the user's full task and relevant context. Tell it to use Computer Use to complete the task and report its findings. For browser work, tell it to use Google Chrome unless the user names another browser. Save the Pi session and record its session ID.

Wait for the agent to finish, then read its full output from the caller's tab. Do not close or focus the caller's pane or tab. Leave the created tab open so the user can inspect its output or continue from the saved session.

Report the agent's findings to the user. End with its Pi session ID and reopen command:

```bash
pi --session <session-id>
```
