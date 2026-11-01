<div align="center">

# pi-send-notification

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Version](https://img.shields.io/badge/version-v1.0.0-blue.svg)
[![GitHub stars](https://img.shields.io/github/stars/pingdongyi/pi-send-notification.svg)](https://github.com/pingdongyi/pi-send-notification)
[![GitHub issues](https://img.shields.io/github/issues/pingdongyi/pi-send-notification.svg)](https://github.com/pingdongyi/pi-send-notification)

A [Pi](https://pi.dev) skill to trigger **local Android system notifications** via the Termux API (`termux-notification`).

</div>

Install from Git:

```bash
pi install git:github.com/pingdongyi/pi-send-notification@v1
```

Or locally:

```bash
pi install ./send-notification-skill
```

Then load the skill in a session:

```
/skill:send-notification
```

## What it does

Sends a local Android notification with a title and optional body text, using `termux-notification`. Supports priority, sound, vibration, action buttons, and an ID for updating an existing notification.

## Example

```bash
termux-notification -t "任务完成" -c "本周报告已生成" --priority high --sound
```

Or with content from stdin:

```bash
echo "详情内容" | termux-notification -t "标题"
```

For an interactive notification with action buttons:

```bash
termux-notification -t "下载完成" -c "点击操作" \
  --button1 "打开文件" --button1-action "termux-open ~/file.zip" \
  --button2 "忽略" --button2-action "termux-toast '已忽略'" \
  --action "termux-open ~/file.zip"
```

## Requirements

- Termux with the [Termux:API](https://wiki.termux.com/wiki/Termux:API) app installed.
- `termux-api` package installed (`pkg install termux-api`).

## License

MIT
