# 推荐 — 个性化书单

## 接口

**PATH：** `/lrts/cgi/lragent/recommend`（GET 或 POST）

**请求参数：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `size` | int | 否 | 条数，默认 5，上限 20 |
| `userId` | int64 | 否 | 已废弃，身份只认 key，传了也会被忽略 |

**返回 `data`：**

| 字段 | 说明 |
|---|---|
| `items[]` | 书单：`entityId` / `entityType`(1书 2节目) / `name` / `cover` / `author` / `reason`（推荐语，可能为空）/ `webUrl` |
| `count` | 条数 |
| `mode` | `rec` 表示真实召回链路 |
| `skillVersion` | 服务端回显的版本 |

## 示例

```bash
curl -sS -G "https://shapi.lanrentingshu.com/lrts/cgi/lragent/recommend" \
  -H "Authorization: Bearer $LRTS_LRAGENT_KEY" \
  --data-urlencode "size=5"
```

```powershell
$h = @{ Authorization = "Bearer $env:LRTS_LRAGENT_KEY" }
Invoke-RestMethod -Uri "https://shapi.lanrentingshu.com/lrts/cgi/lragent/recommend?size=5" -Headers $h
```

## 注意

- 需要 key，未授权会返回 401；
- `items` 为空就如实说「暂无推荐」，不要补造；
- 用户说「换一批」时再调一次即可，不要自己拼别的书单。
