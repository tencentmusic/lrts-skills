# 分类浏览 — 按分类拿一批内容

## 接口

**PATH：** `/lrts/cgi/lragent/discoverChannels`（GET 或 POST），**必须带 key**。

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `channel` | string | 是 | 分类名，最长 20 字，如 悬疑 / 儿童 / 相声 / 评书 |
| `keyword` | string | 否 | 分类内附加关键词，最长 30 字 |
| `size` | int | 否 | 条数，默认 10，上限 20 |

**返回 `data`：**

| 字段 | 说明 |
|---|---|
| `channel` / `keyword` | 回显 |
| `summary` | 模型给的分类导语，可能为空 |
| `items[]` | 与搜索一致：`entityId` / `entityType` / `name` / `author` / `announcer` / `cover` / `plays` / `reason` / `webUrl` |
| `count` | 条数 |
| `partial` | `true` 表示「时间到先给这些」，结果可能不完整 |
| `source` | `llm`=模型生成；`search`=没赶上生成、按分类名搜到的内容 |

## 示例

```bash
curl -sS -G "https://shapi.lanrentingshu.com/lrts/cgi/lragent/discoverChannels" \
  -H "Authorization: Bearer $LRTS_LRAGENT_KEY" \
  --data-urlencode "channel=儿童故事" --data-urlencode "size=5"
```

```powershell
$h = @{ Authorization = "Bearer $env:LRTS_LRAGENT_KEY" }
Invoke-RestMethod -Uri "https://shapi.lanrentingshu.com/lrts/cgi/lragent/discoverChannels?channel=儿童故事&size=5" -Headers $h
```

## 注意

- `partial=true`：开头补一句「结果还在生成，先给你这几本」，不要说得像完整结果；
- `source=search`：`summary` 为空时不要编导语；
- `items` 为空就说「这个分类下暂时没有找到内容」，不要用别的分类/榜单顶替；
- 分类名用用户原话，不要自己改成站内不存在的分类。
