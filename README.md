# codex-claude-resume

A manual workflow for resuming Claude Code tasks with Codex after a usage-limit reset.

**Report the limit → schedule one check → resume the same session → pause again.**

This repo contains instructions and prompt templates, not a standalone app.

## Requirements

- Codex with native scheduling and access to your local Claude Code work.
- Claude Code signed in with your subscription; paid extra usage disabled.
- Your computer awake and online, with Codex and Claude available when the check runs.

## Quick start

### 1. Open this repo in Codex

```bash
git clone https://github.com/YI-XIN-Zhan/codex-claude-resume.git
cd codex-claude-resume
```

### 2. Set up once

Paste into a Codex chat:

```text
Read README.md and templates/codex-resume-prompt.md.
Set up this workflow for my existing Claude Code task.
Confirm the original session and that paid extra usage is off.
Create local state from examples/state.example.json.
Reuse or create a schedule, and keep it paused until I request a resume.
```

### 3. When Claude hits a limit

Return to the same chat and send:

```text
Claude hit its usage limit. Please arrange a resume.
Task: [task name and CLI session or Desktop Code conversation]
Reset time: [date, time, and timezone shown by Claude]
Check one minute after reset, continue the same task, then pause again.
```

Replace the bracketed fields. Codex should confirm the booking or explain why it cannot schedule it.

Report each new limit manually. Only one session can have a pending booking. Codex checks allowance before resuming and preserves drafts, questions, and already running or completed work.

**Cancel:** “Cancel the pending Claude resume.”

**Without scheduling:** After reset, say “Check and resume my Claude task now, without scheduling.”

## Status

A controlled test passed: scheduled Codex wake-up and continuation of the same Claude CLI session in non-interactive mode. **Real quota recovery and Desktop Code automation remain unverified.** See the [test report](docs/testing.md).

Local state and credentials stay on your computer. Setup and scheduled runs still consume model usage. Detailed rules are in the [prompt template](templates/codex-resume-prompt.md).

## License

[MIT](LICENSE). Independent project; not affiliated with OpenAI or Anthropic.
