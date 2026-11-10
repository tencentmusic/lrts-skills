# 收听历史

## 接口

**PATH：** `/lrts/cgi/lragent/history`（GET 或 POST），**必须带 key**。

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `pageSize` | int | 否 | 条数，默认 20，上限 50 |

**返回 `data`：**

| 字段 | 说明 |
|---|---|
| `count` | 条数 |
| `items[]` | `entityId` / `entityType`(1书 2节目) / `name` / `cover` / `author`(主播优先) / `listPos`（上次听到的位置）/ `playSeconds`（累计秒数，可能缺失）/ `modifyTime`（毫秒时间戳）/ `webUrl` |

列表已按最近收听时间倒序，**不要重排**。

## 示例

```bash
curl -sS -G "https://shapi.lanrentingshu.com/lrts/cgi/lragent/history" \
  -H "Authorization: Bearer $LRTS_LRAGENT_KEY" \
  --data-urlencode "pageSize=20"
```

```powershell
$h = @{ Authorization = "Bearer $env:LRTS_LRAGENT_KEY" }
Invoke-RestMethod -Uri "https://shapi.lanrentingshu.com/lrts/cgi/lragent/history?pageSize=20" -Headers $h
```

## 注意

- `items` 为空就说「还没有收听记录」；
- 不要展示 `entityId`、`modifyTime` 这类字段；
- 用户问「听了多久 / 什么题材多」时走 `report.md` 的听书报告，而不是自己汇总历史列表。
