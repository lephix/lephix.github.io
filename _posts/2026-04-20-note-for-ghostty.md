---
title: Note for Ghostty
categories: [Misc]
tags: [ghostty, terminal]
---

# Ghostty 配置笔记

[Ghostty](https://ghostty.org/) 是一款原生、GPU 加速的终端模拟器，由 Mitchell Hashimoto（HashiCorp 联合创始人）开发。配置文件位于 `~/.config/ghostty/config`，采用 `key = value` 的纯文本格式。

## Quick Terminal（Guake 样式下拉终端）

Quick Terminal 是 Ghostty 内置的全局快捷终端面板，类似 Guake / Yakuake 的下拉式终端体验——按一个键唤出，再按一次收起，随时可用。

### 全局热键

```ini
keybind = global:f12=toggle_quick_terminal
```

- `global:` 前缀表示这是一个**系统级全局热键**，即使 Ghostty 不在前台也能触发。macOS 上首次使用需授予辅助功能（Accessibility）权限。
- `toggle_quick_terminal` 动作用于切换 Quick Terminal 的显示/隐藏。系统中同一时间只会存在一个 Quick Terminal 实例。
- Ghostty 默认**不绑定**任何 Quick Terminal 快捷键，必须手动配置。

### 面板位置

```ini
quick-terminal-position = top
```

控制 Quick Terminal 从屏幕的哪个方向滑出。可选值：

| 值 | 说明 |
|---|---|
| `top` | 从屏幕顶部滑下（默认值，经典 Guake 风格） |
| `bottom` | 从屏幕底部滑上 |
| `left` | 从屏幕左侧滑出 |
| `right` | 从屏幕右侧滑出 |
| `center` | 在屏幕中央弹出 |

> macOS 上修改此项后需要**重启 Ghostty** 才能生效。

### 面板尺寸

```ini
quick-terminal-size = 60%,90%
```

格式为 `<主轴尺寸>,<副轴尺寸>`，主/副轴的含义取决于 `quick-terminal-position`：

| 位置 | 主轴（第一个值） | 副轴（第二个值） |
|---|---|---|
| `top` / `bottom` | 高度 | 宽度 |
| `left` / `right` | 宽度 | 高度 |
| `center` | 宽度 | 高度 |

支持两种单位：
- **百分比**：如 `60%`，相对于屏幕尺寸
- **像素**：如 `500px`，绝对像素值

示例中 `60%,90%` 表示：面板高度占屏幕 60%，宽度占屏幕 90%。

### 动画时长

```ini
quick-terminal-animation-duration = 0.15
```

Quick Terminal 滑入/滑出的动画持续时间，单位为**秒**。默认值 `0.2`，设为 `0` 可完全关闭动画。仅 macOS 支持，可在运行时修改即时生效。

### 自动隐藏

```ini
quick-terminal-autohide = true
```

当焦点切换到其他窗口时，是否自动收起 Quick Terminal。可选值：

| 值 | 说明 |
|---|---|
| `true` | 失去焦点时自动隐藏（macOS 默认） |
| `false` | 失去焦点后仍保持显示（Linux/BSD 默认） |

macOS 上默认为 `true`，Linux/BSD 上默认为 `false`。Linux 下全局快捷键需要额外的系统配置，因此默认保持打开直到用户主动关闭。

## 外观与透明度

### 背景透明度

```ini
background-opacity = 0.85
```

终端背景的不透明度，取值范围 `0.0`（完全透明）到 `1.0`（完全不透明），默认 `1.0`。超出范围的值会被自动钳制。建议不要低于 `0.3`，否则终端内容可能难以辨认。

### 背景模糊

```ini
background-blur = macos-glass-regular
```

为终端窗口启用背景模糊效果，配合 `background-opacity` 使用效果更佳。可选值：

| 值 | 说明 |
|---|---|
| `false` | 禁用背景模糊（默认） |
| `true` | 启用背景模糊，使用默认模糊半径（macOS 上为 20） |
| `<数字>` | 指定模糊半径（仅 macOS），如 `20`、`40` |
| `macos-glass-regular` | macOS 原生标准毛玻璃效果，带适度透明感 |
| `macos-glass-clear` | macOS 原生高透明毛玻璃效果 |

> Linux 上仅支持 `true` / `false`，不支持数值和 glass 效果。

### 窗口主题

```ini
window-theme = ghostty
```

控制窗口标题栏和窗口装饰的主题风格。可选值：

| 值 | 说明 |
|---|---|
| `auto` | 根据终端背景色的亮度自动选择亮/暗主题（默认） |
| `system` | 跟随系统主题 |
| `light` | 强制使用亮色主题 |
| `dark` | 强制使用暗色主题 |
| `ghostty` | 使用 Ghostty 自定义主题样式 |

> 在 Linux/GTK 下选择 `ghostty` 时，还可通过 `window-titlebar-background` 和 `window-titlebar-foreground` 自定义标题栏颜色。

### 窗口阴影

```ini
macos-window-shadow = true
```

是否启用 macOS 窗口阴影，默认 `true`。在使用窗口透明度或特定窗口管理器时，设为 `false` 可能视觉效果更好。

## Fish Shell 集成

### 设置默认 Shell

```ini
command = /opt/homebrew/bin/fish
```

指定新建终端时运行的命令/Shell。此配置对所有终端窗口生效（包括新标签页、分屏和 Quick Terminal）。如果不设置，Ghostty 会使用系统默认 Shell。

### `command` vs `initial-command`

```ini
# 所有终端窗口都使用 Fish
command = /opt/homebrew/bin/fish

# 仅首次启动的窗口使用此命令（较少使用）
# initial-command = /opt/homebrew/bin/fish
```

| 配置项 | 作用范围 |
|---|---|
| `command` | 所有终端窗口（新标签页、分屏等） |
| `initial-command` | 仅 Ghostty 启动时创建的第一个终端窗口 |

典型用途：用 `initial-command` 在启动时运行欢迎脚本或特定程序，之后的窗口仍使用 `command` 指定的 Shell。

### Shell Integration

```ini
shell-integration = fish
```

启用指定 Shell 的深度集成功能。Ghostty 会自动向 Shell 注入集成脚本，无需手动安装插件。可选值：

| 值 | 说明 |
|---|---|
| `detect` | 自动检测当前 Shell 并启用集成（默认） |
| `fish` | 启用 Fish 集成 |
| `zsh` | 启用 Zsh 集成 |
| `bash` | 启用 Bash 集成 |
| `elvish` | 启用 Elvish 集成 |
| `nushell` | 启用 Nushell 集成 |
| `none` | 完全禁用 Shell 集成 |

### Shell Integration Features

```ini
shell-integration-features = prompt,cursor,title,path
```

精细控制启用哪些 Shell 集成功能，多个功能用逗号分隔。在功能名前加 `no-` 前缀可禁用该功能。

| 功能 | 默认 | 说明 |
|---|---|---|
| `cursor` | 开启 | 在命令提示符处将光标样式切换为竖线（bar），便于区分输入位置 |
| `title` | 开启 | 根据 Shell 状态（当前命令、目录等）自动更新窗口标题 |
| `path` | 开启 | 向终端报告当前工作目录，支持在新标签页/分屏中继承路径 |
| `sudo` | 关闭 | 在 `sudo` 会话中保留 Shell 集成功能 |
| `ssh-env` | 关闭 | 为 SSH 会话设置环境变量，以在远程主机上保持 Ghostty 特性 |
| `ssh-terminfo` | 关闭 | SSH 连接时自动在远程主机上安装 Ghostty 的 terminfo，使远程终端正确识别 Ghostty |

示例：

```ini
# 启用所有推荐功能 + sudo 集成
shell-integration-features = prompt,cursor,title,path,sudo

# 禁用光标样式变化
shell-integration-features = no-cursor
```

## 完整配置参考

```ini
# ==================== Quick Terminal ====================
keybind = global:f12=toggle_quick_terminal
quick-terminal-position = top
quick-terminal-size = 60%,90%
quick-terminal-animation-duration = 0.15
quick-terminal-autohide = true

# ==================== 外观 ====================
background-opacity = 0.85
background-blur = macos-glass-regular
window-theme = ghostty
macos-window-shadow = true

# ==================== Fish Shell 集成 ====================
command = /opt/homebrew/bin/fish
shell-integration = fish
shell-integration-features = prompt,cursor,title,path
```

## 参考资料

- [Ghostty 官方文档 - 配置参考](https://ghostty.org/docs/config/reference)
- [Ghostty GitHub 仓库](https://github.com/ghostty-org/ghostty)
