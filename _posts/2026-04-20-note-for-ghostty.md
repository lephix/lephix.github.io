---
title: Ghostty + Fish + Starship 终端配置指南
categories: [Misc]
tags: [ghostty, fish, starship, terminal]
---

# Ghostty + Fish + Starship 终端配置指南

[Ghostty](https://ghostty.org/) 是一款原生、GPU 加速的终端模拟器，[Fish](https://fishshell.com/) 是一款开箱即用的智能 Shell，[Starship](https://starship.rs/) 是一个极快的跨 Shell 提示符。三者组合可以打造一个美观、高效、低配置成本的现代终端环境。

---

## Ghostty 配置

配置文件位于 `~/.config/ghostty/config`，采用 `key = value` 的纯文本格式。

### Quick Terminal（Guake 样式下拉终端）

Quick Terminal 是 Ghostty 内置的全局快捷终端面板，类似 Guake / Yakuake 的下拉式终端体验——按一个键唤出，再按一次收起，随时可用。

#### 全局热键

```ini
keybind = global:f12=toggle_quick_terminal
```

- `global:` 前缀表示这是一个**系统级全局热键**，即使 Ghostty 不在前台也能触发。macOS 上首次使用需授予辅助功能（Accessibility）权限。
- `toggle_quick_terminal` 动作用于切换 Quick Terminal 的显示/隐藏。系统中同一时间只会存在一个 Quick Terminal 实例。
- Ghostty 默认**不绑定**任何 Quick Terminal 快捷键，必须手动配置。

#### 面板位置

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

#### 面板尺寸

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

#### 动画时长

```ini
quick-terminal-animation-duration = 0.15
```

Quick Terminal 滑入/滑出的动画持续时间，单位为**秒**。默认值 `0.2`，设为 `0` 可完全关闭动画。仅 macOS 支持，可在运行时修改即时生效。

#### 自动隐藏

```ini
quick-terminal-autohide = true
```

当焦点切换到其他窗口时，是否自动收起 Quick Terminal。可选值：

| 值 | 说明 |
|---|---|
| `true` | 失去焦点时自动隐藏（macOS 默认） |
| `false` | 失去焦点后仍保持显示（Linux/BSD 默认） |

macOS 上默认为 `true`，Linux/BSD 上默认为 `false`。Linux 下全局快捷键需要额外的系统配置，因此默认保持打开直到用户主动关闭。

### 外观与透明度

#### 背景透明度

```ini
background-opacity = 0.85
```

终端背景的不透明度，取值范围 `0.0`（完全透明）到 `1.0`（完全不透明），默认 `1.0`。超出范围的值会被自动钳制。建议不要低于 `0.3`，否则终端内容可能难以辨认。

#### 背景模糊

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

#### 窗口主题

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

#### 窗口阴影

```ini
macos-window-shadow = true
```

是否启用 macOS 窗口阴影，默认 `true`。在使用窗口透明度或特定窗口管理器时，设为 `false` 可能视觉效果更好。

### Shell 集成

在 Ghostty 中指定使用 Fish 作为默认 Shell，并启用深度集成功能。

#### 默认 Shell

```ini
command = /opt/homebrew/bin/fish
```

指定新建终端时运行的 Shell。此配置对所有终端窗口生效（包括新标签页、分屏和 Quick Terminal）。如果不设置，Ghostty 会使用系统默认 Shell。

> `command` 作用于所有窗口；`initial-command` 仅作用于 Ghostty 启动时创建的第一个窗口，适合用来运行欢迎脚本或特定程序。

#### Shell Integration

```ini
shell-integration = fish
shell-integration-features = prompt,cursor,title,path
```

Ghostty 会自动向 Shell 注入集成脚本，无需手动安装插件。

| 功能 | 默认 | 说明 |
|---|---|---|
| `cursor` | 开启 | 在命令提示符处将光标样式切换为竖线（bar），便于区分输入位置 |
| `title` | 开启 | 根据 Shell 状态（当前命令、目录等）自动更新窗口标题 |
| `path` | 开启 | 向终端报告当前工作目录，支持在新标签页/分屏中继承路径 |
| `sudo` | 关闭 | 在 `sudo` 会话中保留 Shell 集成功能 |
| `ssh-env` | 关闭 | 为 SSH 会话设置环境变量，以在远程主机上保持 Ghostty 特性 |
| `ssh-terminfo` | 关闭 | SSH 连接时自动在远程主机上安装 Ghostty 的 terminfo |

---

## Fish Shell 配置

Fish（Friendly Interactive Shell）的配置文件位于 `~/.config/fish/config.fish`。相比 Bash/Zsh，Fish 的特点是**开箱即用**——语法高亮、自动补全、历史搜索等功能无需额外配置。

### 安装

```bash
# macOS
brew install fish

# Debian / Ubuntu
sudo apt install fish

# Arch Linux
sudo pacman -S fish
```

安装后可通过 `fish_add_path` 添加 PATH，无需手动编辑 `config.fish`：

```bash
fish_add_path /opt/homebrew/bin
fish_add_path ~/.local/bin
```

### Abbreviations（缩写）

Fish 的 abbreviation 是比 alias 更好的替代方案——输入缩写后按空格会自动展开为完整命令，在历史记录中保留的是展开后的完整命令，方便回溯。

```fish
# 在终端中直接运行，永久生效
abbr -a g git
abbr -a ga 'git add'
abbr -a gc 'git commit'
abbr -a gco 'git checkout'
abbr -a gd 'git diff'
abbr -a gs 'git status'
abbr -a gp 'git push'
abbr -a gl 'git pull'
abbr -a glog 'git log --oneline --graph'

abbr -a ll 'ls -lah'
abbr -a la 'ls -a'
abbr -a .. 'cd ..'
abbr -a ... 'cd ../..'
```

> Abbreviation 通过 `abbr -a` 命令设置后会自动持久化到 `~/.config/fish/fish_variables`，不需要写入 `config.fish`。

### Fisher 插件管理

[Fisher](https://github.com/jorgebucaran/fisher) 是 Fish 最流行的插件管理器，轻量且快速。

```bash
# 安装 Fisher
curl -sL https://raw.githubusercontent.com/jorgebucaran/fisher/main/functions/fisher.fish | source && fisher install jorgebucaran/fisher
```

常用插件推荐：

```bash
# z - 智能目录跳转（类似 autojump / zoxide）
fisher install jethrokuan/z

# fzf.fish - fzf 集成（Ctrl+R 历史搜索、Ctrl+O 文件搜索、Ctrl+Alt+F 进程搜索）
fisher install PatrickF1/fzf.fish

# done - 长时间运行的命令完成时发送通知
fisher install franciscolourenco/done

# autopair - 自动补全括号、引号
fisher install jorgebucaran/autopair.fish
```

### 常用配置

在 `~/.config/fish/config.fish` 中进行自定义配置：

```fish
# ==================== 环境变量 ====================
set -gx EDITOR vim
set -gx LANG en_US.UTF-8

# ==================== 交互式 Shell 配置 ====================
if status is-interactive
    # 关闭欢迎语
    set -g fish_greeting

    # 初始化 Starship 提示符
    starship init fish | source
end
```

> `status is-interactive` 确保只在交互式会话中执行这些配置，不影响脚本执行。

### 实用内置功能

Fish 自带许多无需配置即可使用的功能：

| 功能 | 说明 |
|---|---|
| **语法高亮** | 命令存在时为蓝色，不存在时为红色，实时反馈 |
| **自动建议** | 根据历史记录和补全候选，以灰色文字显示建议，按 `→` 或 `Ctrl+F` 接受 |
| **Tab 补全** | 按 `Tab` 展示补全候选，支持文件路径、命令参数、Git 分支等 |
| **历史搜索** | `↑` / `↓` 基于当前输入前缀搜索历史，比 `Ctrl+R` 更直觉 |
| **通配符** | `**` 递归匹配，如 `ls **/*.md` 列出所有 Markdown 文件 |

---

## Starship 配置

[Starship](https://starship.rs/) 是一个用 Rust 编写的极快提示符，支持几乎所有 Shell。它通过检测当前目录的上下文（Git 状态、语言版本、包版本等）来动态生成提示符内容。

配置文件位于 `~/.config/starship.toml`。

### 安装

```bash
# macOS
brew install starship

# 通用安装脚本
curl -sS https://starship.rs/install.sh | sh
```

安装后在 Fish 中初始化（已包含在上面的 `config.fish` 中）：

```fish
# ~/.config/fish/config.fish
if status is-interactive
    starship init fish | source
end
```

### 提示符格式

通过 `format` 字段自定义提示符中模块的排列顺序和样式：

```toml
format = """
$directory\
$git_branch\
$git_status\
$python\
$nodejs\
$rust\
$java\
$golang\
$docker_context\
$cmd_duration\
$line_break\
$character"""
```

每个 `$module_name` 对应一个模块，仅在相关上下文存在时才会显示。例如 `$python` 只有在当前目录包含 `.py` 文件或 `pyproject.toml` 时才出现。

### 常用模块配置

#### 目录显示

```toml
[directory]
truncation_length = 3        # 最多显示 3 层目录
truncation_symbol = "…/"     # 截断时的省略符号
home_symbol = "~"            # 用 ~ 代替 /Users/xxx
read_only = " 🔒"            # 只读目录的标识
```

#### Git 分支与状态

```toml
[git_branch]
symbol = " "                # 分支图标
format = "[$symbol$branch(:$remote_branch)]($style) "

[git_status]
format = '([$all_status$ahead_behind]($style) )'
conflicted = "="
ahead = "⇡${count}"
behind = "⇣${count}"
diverged = "⇕⇡${ahead_count}⇣${behind_count}"
untracked = "?${count}"
stashed = "*${count}"
modified = "!${count}"
staged = "+${count}"
deleted = "✘${count}"
```

#### 命令执行时间

```toml
[cmd_duration]
min_time = 2000              # 命令执行超过 2 秒才显示
format = "[$duration]($style) "
style = "yellow"
```

#### 提示符字符

```toml
[character]
success_symbol = "[❯](green)"
error_symbol = "[❯](red)"     # 上一条命令失败时变红
```

#### 语言版本

```toml
[python]
symbol = " "
format = '[${symbol}${pyenv_prefix}(${version} )(\($virtualenv\) )]($style)'

[nodejs]
symbol = " "
format = "[$symbol($version )]($style)"

[rust]
symbol = " "
format = "[$symbol($version )]($style)"

[golang]
symbol = " "
format = "[$symbol($version )]($style)"

[java]
symbol = " "
format = "[$symbol($version )]($style)"
```

#### Docker 上下文

```toml
[docker_context]
symbol = " "
format = "[$symbol$context]($style) "
only_with_files = true        # 仅在目录包含 Docker 相关文件时显示
```

### Starship 预设

Starship 提供了多个开箱即用的预设主题，可以快速获得一个美观的提示符：

```bash
# 查看所有可用预设
starship preset --list

# 应用预设（会覆盖当前配置）
starship preset nerd-font-symbols -o ~/.config/starship.toml
starship preset tokyo-night -o ~/.config/starship.toml
```

> 使用 Nerd Font 相关预设前，需要先安装一款 [Nerd Font](https://www.nerdfonts.com/) 字体，并在 Ghostty 中通过 `font-family` 配置使用。

---

## 完整配置参考

### Ghostty（`~/.config/ghostty/config`）

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

# ==================== Shell 集成 ====================
command = /opt/homebrew/bin/fish
shell-integration = fish
shell-integration-features = prompt,cursor,title,path
```

### Fish（`~/.config/fish/config.fish`）

```fish
# ==================== 环境变量 ====================
set -gx EDITOR vim
set -gx LANG en_US.UTF-8

# ==================== 交互式 Shell 配置 ====================
if status is-interactive
    # 关闭欢迎语
    set -g fish_greeting

    # 初始化 Starship 提示符
    starship init fish | source
end
```

### Starship（`~/.config/starship.toml`）

```toml
format = """
$directory\
$git_branch\
$git_status\
$python\
$nodejs\
$rust\
$java\
$golang\
$docker_context\
$cmd_duration\
$line_break\
$character"""

# ==================== 目录 ====================
[directory]
truncation_length = 3
truncation_symbol = "…/"
home_symbol = "~"
read_only = " 🔒"

# ==================== Git ====================
[git_branch]
symbol = " "
format = "[$symbol$branch(:$remote_branch)]($style) "

[git_status]
format = '([$all_status$ahead_behind]($style) )'
conflicted = "="
ahead = "⇡${count}"
behind = "⇣${count}"
diverged = "⇕⇡${ahead_count}⇣${behind_count}"
untracked = "?${count}"
stashed = "*${count}"
modified = "!${count}"
staged = "+${count}"
deleted = "✘${count}"

# ==================== 命令时间 ====================
[cmd_duration]
min_time = 2000
format = "[$duration]($style) "
style = "yellow"

# ==================== 提示符 ====================
[character]
success_symbol = "[❯](green)"
error_symbol = "[❯](red)"

# ==================== 语言 ====================
[python]
symbol = " "
format = '[${symbol}${pyenv_prefix}(${version} )(\($virtualenv\) )]($style)'

[nodejs]
symbol = " "
format = "[$symbol($version )]($style)"

[rust]
symbol = " "
format = "[$symbol($version )]($style)"

[golang]
symbol = " "
format = "[$symbol($version )]($style)"

[java]
symbol = " "
format = "[$symbol($version )]($style)"

# ==================== Docker ====================
[docker_context]
symbol = " "
format = "[$symbol$context]($style) "
only_with_files = true
```

## 参考资料

- [Ghostty 官方文档 - 配置参考](https://ghostty.org/docs/config/reference)
- [Ghostty GitHub 仓库](https://github.com/ghostty-org/ghostty)
- [Fish Shell 官方文档](https://fishshell.com/docs/current/)
- [Fisher 插件管理器](https://github.com/jorgebucaran/fisher)
- [Starship 官方文档](https://starship.rs/config/)
- [Nerd Fonts](https://www.nerdfonts.com/)
