# 懒人听书 skill

支持书单推荐、有声书搜索、各类榜单、收听历史查询、单本书/节目 AI 解读、分类浏览与听书报告。

全部能力都通过 HTTP 接口调用，不依赖 shell 脚本，macOS / Linux / Windows 均可使用。

## 配置

### 获取 API Key

1. 访问 <https://www.lanrentingshu.com/info/skill-key>，登录后生成并复制 API Key（形如 `lsk_xxx`）。
2. 把 key 放到下面任一位置，skill 会自动读取：
   - 环境变量 `LRTS_LRAGENT_KEY`
   - 本地文件（macOS/Linux）`~/.config/lrts-lragent/key`
   - 本地文件（Windows）`%USERPROFILE%\.config\lrts-lragent\key`
3. 不要写进代码、不要提交到 git；泄露请立刻到授权页重置，旧 key 会立即失效。

Key 有效期 30 天，到期重新授权即可。

```bash
# macOS / Linux
export LRTS_LRAGENT_KEY=lsk_xxx
```

```powershell
# Windows PowerShell
[Environment]::SetEnvironmentVariable("LRTS_LRAGENT_KEY","lsk_xxx","User")
```

## 使用示例

- "推荐几本书" / "猜你喜欢" / "换一批"
- "看看排行榜" / "热播榜前10" / "悬疑榜呢"
- "这个榜单是什么" / "榜单有多少本"
- "搜一下剑来" / "有没有盗墓笔记这本书"
- "我的收听历史" / "我最近在听什么"
- "解读一下这本书" / "这本书讲什么" / "适合谁听"
- "悬疑分类有什么好听的" / "推荐几本儿童故事" / "来点相声"
- "我的听书报告" / "我听了多久" / "我都听了些什么"

## 功能

| 能力 | 说明 | 文档 |
|---|---|---|
| 推荐 | 按账号生成个性化书单，可指定条数 | `recommend.md` |
| 搜索 | 按关键词搜有声书 / 节目；搜不到时免费优先 | `search.md` |
| 榜单 | 查看指定榜单的前 N 名，支持翻页 | `charts.md` |
| 榜单详情 | 榜单名称、排序规则、更新时间 | `charts.md` |
| 收听历史 | 最近听过的书与节目，按时间倒序 | `history.md` |
| AI 解读 | 对单本书/节目生成内容概述、亮点和适合人群 | `interpret.md` |
| 分类浏览 | 按分类拿一批有声书/节目 | `channels.md` |
| 听书报告 | 最近收听记录聚合：本数、总时长、收听天数、偏好题材、最久的书 | `report.md` |

推荐、收听历史、AI 解读、分类浏览、听书报告需要授权；搜索、榜单可匿名使用但能力有限。

## 目录结构

```
.
├── LICENSE
├── README.md
└── lanrentingshu/          # skill 本体
    ├── SKILL.md            # 触发条件、调用规范、输出要求
    ├── recommend.md        # 推荐
    ├── search.md           # 搜索（含免费优先策略）
    ├── charts.md           # 榜单 + 榜单详情
    ├── history.md          # 收听历史
    ├── interpret.md        # AI 解读
    ├── channels.md         # 分类浏览
    └── report.md           # 听书报告
```

## 安全约定

- 只读取 `LRTS_LRAGENT_KEY` 或本地 key 文件，不扫描其它环境变量或凭据；
- 请求只发往懒人听书自有域名，且强制 https；地址被改到外部主机时停止调用；
- 全部能力都是只读查询，不涉及下单、支付、改资料等写操作。

## 版本

当前版本：**0.1.2**

## License

CC BY 4.0
