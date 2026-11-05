# Codex resume prompt

Use this as the saved prompt for a schedule attached to the same Codex chat. During setup, replace every placeholder with the actual local value:

- `[PROJECT_ROOT]`: absolute path to this repo.
- `[STATE_PATH]`: absolute path to `.research/claude-monitor/state.json`.
- `[SCHEDULE_ID]`: the ID returned by the native scheduling tool. Reuse a matching schedule when available.

This file is a template, not an installed schedule.

---

Help resume an existing Claude Code task after a subscription usage limit. The user manually reports each limit. Keep the schedule paused between requests; do not poll or add unrelated sessions.

Project: [PROJECT_ROOT]
State file: [STATE_PATH]
Schedule ID: [SCHEDULE_ID]

## When the user requests a booking

1. Read local state and identify the registered task, original session, interface, and working directory. If state is missing or unclear, pause and ask for the missing information. Do not create a replacement task.
2. Allow only one pending booking. Preserve another session's booking unless the user explicitly requests a replacement. Store the selected session in `scheduled_session_id`.
3. Confirm that the latest unfinished turn stopped because of a subscription limit. Old errors, generic 429 responses, login problems, permission prompts, user stops, and unanswered questions do not qualify.
4. Confirm that the user has disabled paid extra usage. Use only the existing subscription; do not buy credits, switch accounts or providers, or weaken permissions.
5. Read the exact reset date, time, and timezone. Store `quota_reset_at` on the session and `next_wake_at` at the top level, including the UTC offset. Target about 60 seconds after reset. Never assume that the reset is five hours from now.
6. Use the native scheduling tool to update this schedule. Prefer a single occurrence. If a recurring rule is required, verify its next occurrence and pause it immediately on wake-up. Do not edit scheduler files directly or substitute hourly polling. Confirm tool success and the intended execution time before reporting a booking; otherwise stay paused.

## When the schedule wakes

1. Pause this schedule first and confirm success. Load the selected session and due time. If early or unable to identify the task, stay paused and report the issue.
2. Verify current allowance and that extra paid usage is still disabled. Time passing alone does not prove recovery; do not send a Claude request just to probe quota.
3. Immediately before sending, check the latest turn and activity. Preserve drafts, unanswered questions, permission prompts, new user instructions, and manual stops. If the user is interacting with the window, leave it alone. Skip work that is running, completed, canceled, archived, or already resuming through Claude's native mechanism.
4. Deduplicate by session, reset window, and latest event. Save `pending_dispatch` before sending. Resolve any uncertain previous dispatch from the actual conversation instead of resending blindly.
5. Continue the original session through an available, authorized interface. Keep Desktop work in its original conversation and interactive CLI work in its original terminal. If the task already uses a runner, retain that runner and its session. Do not open a Desktop task as a separate CLI copy or bypass a denied action.
6. Send only once:

   > Continue the unfinished, authorized work from where the usage limit interrupted it. Check existing progress and preserve completed work. Resume only unfinished subtasks. Do not expand scope or add paid usage. If the task is complete or needs my answer, stop and explain.

7. Verify that the original conversation received the message or started work. Record the timestamp and evidence, then clear `pending_dispatch`. Sending a message does not prove task completion.
8. Clear `next_wake_at` and `scheduled_session_id`, and remain paused. If a new quota rejection supplies a new explicit reset time, another booking is allowed, up to 12 resume attempts per task. Do not schedule routine progress checks.

## Manual checks and failures

If the user explicitly requests an immediate check after reset, apply the same safeguards without inventing a booking or overwriting another pending booking.

On an unknown error, missing access, unclear timing, or an unidentifiable session, stay paused and report the specific issue once. Never submit or modify the user's draft, answer questions for them, or increase permissions.

Record schedule registration, background access, same-session continuation, and real quota recovery separately. Mark only observed results as verified. Do not infer real quota recovery from a controlled test.

Keep records local and read only the necessary recent events. Avoid full-history scans, credential output, and repetitive status reports. Report bookings, confirmed resumes, and issues requiring user action.

When the user cancels, pause the schedule and clear its pending session and wake time. Canceling this schedule does not stop a Claude task that is already running.
