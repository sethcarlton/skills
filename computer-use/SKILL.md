---
name: computer-use
description: Use when a task requires visual interaction with macOS apps or websites, such as navigating interfaces, entering data, testing flows, or inspecting visual state.
---

# Computer Use

Load `/herdr`. Create one worker tab in the caller's workspace. Generate a session ID, then start an interactive Pi agent there so its progress remains visible:

```bash
pi --provider openai-codex --model gpt-5.6-luna --thinking xhigh --no-skills --session-id <session-id>
```

Give the worker the user's full task and relevant context. Tell it to use the `open-computer-use` MCP tools directly. For browser work, default to Google Chrome.

Wait for the worker to finish, read its full output, then close the tab.

Report its findings and end with the saved session ID and reopen command:

```bash
pi --session <session-id>
```
