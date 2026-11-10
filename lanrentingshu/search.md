# 搜索 — 有声书 / 节目

## 接口

**PATH：** `/lrts/cgi/lragent/search`（GET 或 POST）

**请求参数：**

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `keyword` | string | 是 | 搜索词（书名/题材），空则报错 |
| `pageSize` | int | 否 | 默认 10，上限 20 |
| `pageNo` | int | 否 | 0 基页码，默认 0 |
| `type` | string | 否 | 只支持 `"0"`（书/节目）；`"1"`（主播/作者）**暂时关闭**，传了会提示不支持 |

**返回 `data`：**

| 字段 | 说明 |
|---|---|
| `items[]` | `entityId` / `entityType` / `name` / `author` / `announcer` / `cover` / `plays` / `reason` / `webUrl` |
| `count` | 条数 |
| `keyword` / `pageNo` / `pageSize` / `type` | 回显 |

## 示例

```bash
curl -sS -G "https://shapi.lanrentingshu.com/lrts/cgi/lragent/search" \
  -H "Authorization: Bearer $LRTS_LRAGENT_KEY" \
  --data-urlencode "keyword=白眉大侠" --data-urlencode "pageSize=5"
```

```powershell
$h = @{ Authorization = "Bearer $env:LRTS_LRAGENT_KEY" }
Invoke-RestMethod -Uri "https://shapi.lanrentingshu.com/lrts/cgi/lragent/search?keyword=白眉大侠&pageSize=5" -Headers $h
```

## 免费优先策略（搜不到时）

平台搜索只返回免费可听内容，付费/会员书可能搜不到。`items` 为空时按顺序处理：

1. 再搜一次「`<原词>免费`」，只保留名称/主播/作者**确实包含原词**的结果 → 有则说「《X》只有免费版本，这几个可以听」；
2. 仍没有 → 搜「`全书免费`」拿平台免费内容 → 说「没搜到《X》，先给你几本平台免费内容」，需要时可引导到懒人听书官网；
3. 通用免费内容也没有 → 如实说没找到。

要求：

- 不要把免费兜底结果说成用户要找的那本，也不要编造作者/简介；
- 主播/作者搜索暂时下线，用户要搜主播时直接说明，不要硬调 `type=1`。
