# Test report

**Passed: manual start, scheduled Codex wake-up, and continuation of the same Claude CLI session.**

This controlled test ran on November 5, 2026, using macOS, Claude Code CLI `2.1.316`, and Haiku through an existing subscription. Extra paid usage was disabled. The task required only text output; model tools and extra MCP connections were disabled.

## Timeline

All times use `Asia/Taipei` (UTC+08:00).

| Event | Time |
| --- | --- |
| First-stage message recorded | 16:11:18.082 |
| Scheduled wake-up | 16:15:00 |
| Actual native heartbeat | 16:15:06.660 |
| Second-stage message recorded | 16:16:25.441 |
| Second stage completed | 16:16:27.823 |

The observed wake-up delay was 6.66 seconds. This is a measurement, not a timing guarantee. Codex still needed time to check state and start Claude after waking.

## What was verified

The first stage calculated `37 × 41 = 1517` and stopped. Its instructions also supplied a random verification token and left a second stage unfinished: add `23` and return the token.

The native scheduler woke an existing Codex chat. Codex used `--resume` with the original Claude session and requested the unfinished second stage. It did not repeat the token, calculation inputs, or expected answer.

Checks confirmed:

- Both CLI results used the same `session_id`.
- The second stage returned `1540` and the correct original token.
- The transcript contained exactly the two intended user messages.
- The CLI exited successfully without an error result.
- The original schedule settings were restored and left paused.

## Input fix

An earlier run accidentally passed its Python launch script through inherited standard input. It was excluded from the clean-input result.

Both stages were repeated with `stdin=subprocess.DEVNULL`. The transcript then matched the intended prompts exactly.

## Limits

This was a separate, non-interactive CLI test using test-specific instructions. Usage was about 31%; **recovery after a real quota limit was not tested**.

Desktop Code automation and input into an existing interactive terminal remain unverified. The original research conversation and its unsent draft were untouched.

Private transcripts, account details, session IDs, and local paths remain outside this repository.
