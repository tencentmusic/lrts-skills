# 听书报告 — 最近收听记录聚合

## 接口

**PATH：** `/lrts/cgi/lragent/listenReport`（GET 或 POST），**必须带 key**。

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `size` | int | 否 | 参与统计的最近记录条数，默认 50，上限 50 |

**返回 `data`：**

| 字段 | 说明 |
|---|---|
| `scope` | 统计范围说明，如「最近 50 条收听记录（非全量历史）」 |
| `size` / `bookCount` | 参与统计条数 / 去重后的书（节目）数 |
| `totalSeconds` | 累计播放时长（秒） |
| `listenDays` | 有收听记录的自然日数量（东八区） |
| `topCategories[]` | 偏好题材 TOP3：`name` / `count` |
| `topBooks[]` | 听得最久 TOP5：`entityId` / `name` / `playSeconds` / `webUrl` |
| `latest` | 最近一次收听 |
| `firstTime` / `lastTime` | 这批记录的最早、最晚收听时间（毫秒时间戳） |
| `items[]` | 明细，字段同收听历史 |

## 示例

```bash
curl -sS -G "https://shapi.lanrentingshu.com/lrts/cgi/lragent/listenReport" \
  -H "Authorization: Bearer $LRTS_LRAGENT_KEY" \
  --data-urlencode "size=50"
```

```powershell
$h = @{ Authorization = "Bearer $env:LRTS_LRAGENT_KEY" }
Invoke-RestMethod -Uri "https://shapi.lanrentingshu.com/lrts/cgi/lragent/listenReport?size=50" -Headers $h
```

## 输出格式

```markdown
## 我的听书报告

- 一共听过 **N** 本，累计 **X 小时 Y 分钟**
- 有记录的收听天数：**D 天**
- 最近在听：《书名》

**偏好题材**
- 悬疑（6 本）

**听得最久**
| # | 书名 | 时长 | 链接 |
|---|---|---|---|
```

## 注意

- `totalSeconds` 换算成小时/分钟，不要直接甩秒数；
- 开头点明这是「最近 N 条记录」的范围，别包装成年度/月度报告；
- `topBooks` / `topCategories` 为空就说「记录太少，还看不出偏好」，不要编造。
