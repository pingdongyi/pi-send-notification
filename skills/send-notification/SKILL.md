---
name: send-notification
description: Trigger a local Android system notification via the Termux API (termux-notification). Use when the user asks to be notified, wants a reminder/alert/push on the device, or wants a notification sent (通知/提醒/推送/弹通知). Supports title, content, priority, sound, vibration, buttons, and an ID for updates.
compatibility: Requires Termux with the termux-api package installed and `termux-notification` on PATH.
---

# Send Notification (Termux API)

Trigger a local Android system notification using the Termux API command `termux-notification`.

## When to use

Use this skill when the user asks to:
- "notify me", "send a notification", "remind me", "alert me" (通知我 / 提醒我 / 推送消息 / 弹通知)
- deliver a result or update as a device notification after a long task finishes
- send a reminder with a title and optional body text

## How it works

Run the `termux-notification` command. `termux-notification` requires the Termux API app; content can be passed via `-c/--content` or read from stdin.

## Steps

1. Determine the notification title (`-t/--title`) and content (`-c/--content`) from the user's request. Content may also be piped from stdin.
2. Build the command with the desired options. Common options:
   - `-t/--title <text>`: notification title (bold headline).
   - `-c/--content <text>`: notification body text.
   - `-i/--id <id>`: notification id; reusing it overwrites the previous notification with the same id (useful for progress/status updates).
   - `--priority <high|low|max|min|default>`: notification priority.
   - `--sound`: play a sound.
   - `--vibrate <pattern>`: e.g. `500,1000,200`.
   - `--ongoing`: pin the notification (not dismissible).
   - `--alert-once`: don't alert again when the notification is edited.
   - `--group <group>`: group related notifications together.
   - `--button1/2/3 <text>` and `--button1/2/3-action <action>`: add action buttons.
   - `--action <action>`: action to run when the notification is pressed.
3. Run the command, e.g.:
   `termux-notification -t "任务完成" -c "本周报告已生成" --priority high --sound`
   Or with content from stdin:
   `echo "详情内容" | termux-notification -t "标题"`
4. For interactive notifications, add action buttons so the user can act directly from the notification. Buttons execute an action when pressed. Examples:
   - Open a file: `--button1 "打开文件" --button1-action "termux-open ~/file.zip"`
   - Dismiss with feedback: `--button2 "忽略" --button2-action "termux-toast '已忽略'"`
   - A full interactive example:
     ```
     termux-notification -t "下载完成" -c "点击操作" \
       --button1 "打开文件" --button1-action "termux-open ~/file.zip" \
       --button2 "忽略" --button2-action "termux-toast '已忽略'" \
       --action "termux-open ~/file.zip"
     ```
   - The `--action` option sets the action for pressing the notification body itself; `--on-delete` runs an action when it is cleared. Multiple buttons are supported (`--button1/2/3` with matching `--button1/2/3-action`).
5. Report the outcome to the user. If the command fails (e.g. Termux API app not installed / permission denied), surface the error and suggest running `termux-setup` or installing the Termux:API app.

## Related commands (for completeness)

- `termux-notification-list`: list current notifications.
- `termux-notification-remove [id]`: remove a notification (by id, or all if no id).

## Notes

- The `title` is the primary headline; keep it short and meaningful. Put details in `content`.
- Notifications are local to the device. Do not confuse this with remote messaging (e.g. Feishu/MCP notification servers).
- If the user asks to send a remote message instead (e.g. to Feishu), this skill does not apply; use the appropriate MCP/notification channel.
