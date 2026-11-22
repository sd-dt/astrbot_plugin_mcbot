# astrbot_plugin_mcbot

> AstrBot 插件：用 QQ 管理 Minecraft 挂机机器人（yamb）。

管理员私聊 `!mcbot ...` 即可上线/下线、热切换账号、执行游戏内指令、发送连点，以及开关 AI 聊天。
插件只负责指令解析与权限校验，真正的动作通过本机 HTTP 控制器（`controller.js`，默认 `127.0.0.1:15100`）转发给 yamb。

配套仓库：[sd-dt/yamb-plus](https://github.com/sd-dt/yamb-plus)（控制器、监控、部署脚本）。

## 功能

- `!mcbot start|stop|set|enable|disable|toggle <账号>`：热上/下号，不打扰其它在线账号
- `!mcbot say <文本>`：以指定 MC 账号发言
- `!mcbot <账号> <游戏指令>`：转发到游戏内
- `!mcbot loop ...`：定时循环指令（最小 1 秒，最大 1 小时）
- `!mcbot pay ...`：每日转钱任务管理（需配合 `pay-daily.js`）
- `!qqbot start|stop`：AI 聊天总开关（省 token）
- `/猪`：贴 ㊗️（仅指定主人可用，带黑名单）
- `!mcbot help`：从 `help_catalog.json` 生成的指令目录

管理员、主人、黑名单全部通过环境变量配置，代码里不含任何 QQ 号或密钥。

## 安装

1. 把本目录放到 AstrBot 的插件目录：

```bash
git clone https://github.com/sd-dt/astrbot_plugin_mcbot <AstrBot数据目录>/plugins/astrbot_plugin_mcbot
```

2. 配置环境变量（AstrBot 容器或 systemd 单元）：

```bash
MCBOT_ADMINS=<管理员QQ,逗号分隔>
MCBOT_OWNERS=<主人QQ,逗号分隔>
MCBOT_PIG_OWNER=<可用 /猪 的QQ>
MCBOT_PIG_BLACKLIST=<黑名单QQ,逗号分隔>
MCBOT_CONTROLLER_KEY=<与 controller.js 的 CTRL_KEY 一致（也接受 CTRL_KEY）>
```

未设置时对应列表为空、`CONTROLLER_KEY` 为 `change-me`，插件仍可加载但无法通过控制器鉴权。

3. 重启 AstrBot。

## 依赖

- AstrBot（`astrbot.api` / `astrbot.core.message.components`）
- 本机运行的 `controller.js`（见 yamb-plus 仓库 `controller/`）
- 可选：`pay.json`、`state.json` 等运行时数据由插件自行生成，不要提交到仓库

## 说明

- 本插件为自用项目，接口围绕当前部署的 yamb 定制，未做通用化。
- 本仓库版本已脱敏：真实 token、密钥、QQ 号均已移除，运行时从环境变量读取。

## License

MIT
