# 输入端接口

输入由代表操作的**单个**数据段组成，对于不同操作，所包含的数据不同，举例如下：

```json
{
    "qq": 2711644761,
    "type": "construct",
    "node_id": "2a",
    "word": "这是接龙词"
}
```

每个数据段都必须包含qq号与`type`字段，标明操作类型。对于不同的操作，其余输入如下。

# 可使用的操作及包含的数据

## construct 接龙

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `node_id` | string | 接龙母节点的id号 |
| `word` | string | 接龙词 |

## use_card 使用手牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | 欲使用卡牌的id号 |
| `data` | string | 欲使用卡牌的额外信息 |

## use_equipment 使用装备

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | 欲使用装备的id号 |

## use 使用物品

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `name` | string | 欲使用物品的名字 |

## discard 弃牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `cards` | `list[ProtocolData]` | 由卡牌组成的列表，`type: "card"` |

## draw 抽卡

| 字段名 | 数据类型 | 默认值 | 说明 |
| ----- | ------- | ---- | ---- |
| `num` | integer | 1 | 抽卡张数 |

## check 查询

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `arg` | string | 查询内容 |

可查询内容包含：

| 内容名 | 说明 |
| ----- | ---- |
| keyword_pool | 查询当前奖励词池大小。 |
| begin_pool | 查询当前起始词池大小。 |
| hidden_keyword_pool | 查询当前隐藏奖励池大小。 |
| card_pool | 查询当前卡池总卡数。 |
| keyword | 查询当前奖励词。 |
| active | 查询当前可以接的词。 |
| status | 查询自己当前状态。 |
| global_status | 查询当前全局状态。 |
| profile | 查询自己当前资料。 |
| global_profile | 查询全局资料。 |
| hand_cards | 查询自己当前手牌。 |
| equipments | 查询自己当前装备。 |
| quest | 查询自己手牌中的任务之石的任务。 |
| maj | 查询自己的麻将。 |
| jibi | 查询自己的击毙数。 |
| shop | 查询可购买项目。 |
| bingo | 查询bingo活动进度。 |

## buy 购买商品

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | 商品id |

## buy_event 购买活动商品

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | 活动商品id |

## fork 分叉

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `node_id` | string | 分叉节点id |

## choose 选择

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `object` | string | 需要选择的物品，与output中的choose相同，见下表 |
| `chosen` | `list[data]` | 选择到的卡牌列表，在没有可选择的卡牌时返回空列表 |

| `object` | 说明 | `chosen`代表 |
| -------- | ---- | ---------- |
| `hand_card` | 手牌 | 手牌信息，`type: "card"` |
| `card` | 卡牌列表 | 卡牌id号 |
| `player` | 玩家 | 玩家qq号 |
| `hand_maj` | 手中的麻将牌 | 麻将牌的第几张 |

## 管理相关

### add_begin 添加起始词

### add_keyword 添加奖励词

### add_hidden 添加隐藏奖励词

### compensate 发放超新星

### kill 击毙
