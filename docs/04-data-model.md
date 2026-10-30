# 04 · 数据模型

最小可用实现只需要三个文件。

## 对话（Corridor 的载体）

```json
{
  "turns": [
    { "from": "owner", "via": "channel-a", "text": "…", "at": 1793364600000 },
    { "from": "her",   "via": "channel-a", "text": "…", "at": 1793364610000 }
  ],
  "topics": [
    { "text": "那只猫", "at": 1793364600000, "via": "channel-a" }
  ],
  "questions": [
    { "text": "拍没拍下来", "at": 1793364610000, "via": "channel-a", "answered": false }
  ],
  "pending": [
    { "text": "明天交房租", "by": "owner", "due": "2026-10-31" }
  ],
  "lastChannel": "channel-a",
  "lastAt": 1793364610000,
  "switchedFrom": "",
  "switchedAt": 0
}
```

要点：

- `turns` 只保留最近 40 条，更早的交给屋
- `via` 是平台标识，用于 R5 署名和"别混淆"
- `switchedFrom` 只在跨室时写入，注入后清空（用一次就消耗）

## 记忆节点（House 的载体）

```json
{
  "id": "n_8f3a",
  "kind": "fact | event",
  "topic": "工作班次",
  "text": "每周一三五，早九晚六",
  "strength": 1.0,
  "halfLifeDays": 30,
  "at": 1793364600000,
  "lastAt": 1793364600000,
  "seen": 3,
  "archived": false
}
```

要点：

- `kind` 很重要：**事实按主题覆盖，事件永不覆盖**
  （换工作了，旧班次作废；但"今天看见一只猫"不该被明天的猫顶掉）
- `strength` 与 `halfLifeDays` 决定衰减速度
- `seen` 用于"被想起就加固"（间隔重复）

## 自我状态（Self）

```json
{
  "room": "channel-a",
  "roomAt": 1793364610000,
  "thought": "想问他那只猫长什么样",
  "thoughtAt": 1793364000000,
  "startedAt": 1793300000000
}
```

要点：

- `thought` 不必频繁更新（几小时一次即可）
- `room` 用于生成"你刚从 X 过来"的判断

## 最小实现规模

```
三个 JSON 文件 + 一个检索函数 + 一个渲染函数
不到 800 行代码
```

不需要向量数据库，不需要图数据库。
需要时再上——先用最简单的结构把"同一个屋檐下"跑通。
