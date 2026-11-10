# AI 解读 — 单本书 / 节目

## 接口

**PATH：** `/lrts/cgi/lragent/interpret`（GET 或 POST），**必须带 key**。

| 参数 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `entityId` | int64 | 是 | 书籍/节目 ID（搜索结果里的 `entityId`） |
| `entityType` | int | 否 | `1`=书籍（默认），`2`=节目 |
| `style` | string | 否 | `brief`（默认，概述≤120字、亮点/人群各≤3条）/ `detailed`（≤300字、各≤5条） |
| `question` | string | 否 | 追问，例如「适合谁听」「有没有感情线」 |

**返回 `data`：**

| 字段 | 说明 |
|---|---|
| `name` / `author` | 书名与主播 |
| `summary` | 内容概述 |
| `highlights[]` | 亮点 |
| `suitableFor[]` | 适合人群 |
| `disclaimer` | 固定为「AI生成，仅供参考」 |

## 示例

```bash
curl -sS -G "https://shapi.lanrentingshu.com/lrts/cgi/lragent/interpret" \
  -H "Authorization: Bearer $LRTS_LRAGENT_KEY" \
  --data-urlencode "entityId=24186919" --data-urlencode "style=brief"
```

```powershell
$h = @{ Authorization = "Bearer $env:LRTS_LRAGENT_KEY" }
Invoke-RestMethod -Uri "https://shapi.lanrentingshu.com/lrts/cgi/lragent/interpret?entityId=24186919&style=brief" -Headers $h
```

## 输出格式

```markdown
## 《书名》

**内容概述**
……

**亮点**
- ……

**适合人群**
- ……

> AI生成，仅供参考
```

## 注意

- 只有 `entityId`、没有书名时，先用 `search.md` 搜到再解读；不要自己猜 ID；
- 只用返回的 `summary` / `highlights` / `suitableFor` 组织回答，不补充资料外的剧情、评价、播放量；
- 用户问「值不值得听」用亮点+适合人群客观回答，不下绝对结论；
- 一次对话同一本书最多解读一次，用户追问细节可以再解一次。
