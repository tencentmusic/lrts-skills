# 榜单 — 列表与详情

## 榜单列表

**PATH：** `/lrts/cgi/lragent/rankings`（GET 或 POST）

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `rankId` | int64 | 否 | 榜单 ID，默认 14680（综合热播榜） |
| `start` | int | 否 | 起始位置，默认 0 |
| `size` | int | 否 | 条数，默认 10，上限 20 |

返回 `data`：`rankId` / `start` / `size` / `count` / `items[]`
（`order` 名次 / `entityId` / `entityType` / `name` / `cover` / `author` / `webUrl`）。

## 榜单详情

**PATH：** `/lrts/cgi/lragent/rankingsdetail`（GET 或 POST）

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `rankId` | int64 | 否 | 同上，默认 14680 |

返回 `data`：`rankId` / `name` / `originName` / `rankType` / `groupType` / `state` /
`entityCount`（榜内资源数，可能为 0）/ `ruleRemark`（排序规则）/ `ruleUrl` / `updateTime`。

## 常用 rankId

| rankId | 榜单 |
|---|---|
| 14680 | 综合热播榜 |
| 14689 | 悬疑榜（悬疑推理） |
| 14698 | 儿童榜 |

## 示例

```bash
curl -sS -G "https://shapi.lanrentingshu.com/lrts/cgi/lragent/rankings" \
  -H "Authorization: Bearer $LRTS_LRAGENT_KEY" \
  --data-urlencode "rankId=14689" --data-urlencode "size=10"
```

```powershell
$h = @{ Authorization = "Bearer $env:LRTS_LRAGENT_KEY" }
Invoke-RestMethod -Uri "https://shapi.lanrentingshu.com/lrts/cgi/lragent/rankings?rankId=14689&size=10" -Headers $h
```

## 注意

- 问「榜单前 N / 名次」用 `/rankings`；问「这个榜单是什么 / 有多少本 / 规则」用 `/rankingsdetail`；
- `msg=榜单不存在` 说明 rankId 无效或已下线，如实反馈。
