# 输出端接口

输出由代表响应的数据段组成的list构成。此接口兼用于写log，与保存统计数据。

每个数据段都必须包含`type`字段，标明输出类型，以及`qq`字段，标明玩家。对于不同的输出，其余参数如下。

# 可能的输出及包含的数据

## succeed 成功

没有作用。

## failed 失败

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `error_code` | integer | 异常代码，表示失败原因 |

| `error_code` | 说明 | 是否需要`error_msg` |
| ------------ | ---- | ----------------- |
| -1 | unintended error | 是 |
| 0 | 运行时错误 | 是 |
| 1\*\* | 接龙类 | - |
| 100 | 节点未找到 | 否 |
| 101 | 节点已分叉，并不可再次分叉 | 否 |
| 102 | 节点不是活动节点，且不可分叉 | 否 |
| 110 | 接龙词包含非法字符 | 否 |
| 120 | 未达到接龙间隔 | 否 |
| 121 | 无法接龙 | 是，见[接龙树节点](#node 接龙树节点) |
| 190 | 手牌超出上限，不允许接龙 | 否 |
| 2\*\* | 使用类 | - |
| 200 | 未找到卡牌 | 否 |
| 201 | 卡牌不在手牌内 | 否 |
| 202 | 卡牌不满足使用要求 | 否 |
| 210 | 未找到装备 | 否 |
| 211 | 玩家未拥有该装备 | 否 |
| 212 | 装备不满足使用要求 | 否 |
| 219 | 手牌超出上限，不允许使用装备 | 否 |
| 220 | 未找到物品 | 否 |
| 221 | 玩家未拥有该物品 | 否 |
| 222 | 物品不满足使用要求 | 否 |
| 223 | 物品不足，无法使用 | 否 |
| 229 | 手牌超出上限，不允许使用物品 | 否 |
| 3\*\* | 抽卡类 | - |
| 300 | 抽卡券不足 | 否 |
| 390 | 手牌超出上限，不允许抽卡 | 否 |
| 4\*\* | 购买类 | - |
| 400 | 商品未找到 | 否 |
| 410 | 击毙不足 | 否 |
| 411 | 活动pt不足 | 否 |
| 420 | 标记的雷不合法 | 否 |
| 421 | 提交的起始词不合法 | 否 |
| 422 | 提交的奖励词不合法 | 否 |
| 430 | 回溯的节点是根节点 | 否 |
| 431 | 回溯的节点不是活动节点 | 否 |
| 440 | 购买本商品次数用尽 | 否 |
| 441 | 不满足购买本商品的要求 | 否 |
| 450 | 起始词池已空 | 否 |
| 451 | 奖励词池已空 | 否 |
| 452 | 隐藏奖励词池已空 | 否 |
| 5\*\* | 弃牌类 | - |
| 500 | 未找到卡牌 | 否 |
| 501 | 卡牌不在手牌内 | 否 |
| 502 | 卡牌不满足弃牌要求 | 否 |
| 510 | 未在手牌超出上限时使用 | 否 |
| 6\*\* | 分叉 | - |
| 600 | 节点未找到 | 否 |
| 601 | 节点已分叉，并不可再次分叉 | 否 |
| 7\*\* | 查询 | - |
| 700 | 未找到查询目标 | 否 |
| 9\*\* | 后台操作相关 | - |

## choose_failed 选择失败

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `reason` | string | 原因 |

| `reason` | 说明 |
| -------- | ---- |
| `not_active` | 玩家非活跃 |
| `cant_choose` | 没有可选择的对象 |

## response_invalid 选择对象不符合要求

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `error_code` | integer | 异常代码，表示失败原因 |
| `error_msg` | string | 其他原因说明，有时有用 |

| `error_code` | 说明 | 是否需要`error_msg` |
| ------------ | ---- | ----------------- |
| 0 | 响应类型不合规 | 否 |
| 1\*\* | 选择类 | - |
| 100 | 选择数不在范围内 | 否 |
| 110 | 选择手牌不在手牌列表中 | 否 |

## choose 选择

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `object` | string | 需要选择的物品，见下表 |
| `can_choose` | `list[integer]` | 在`object`是`hand_card`时使用，代表手牌的第几张可选择 |
| `card_list` | `list[integer]` | 在`object`是`card`时使用，代表需要从中选择的卡牌列表 |
| `min` | integer | 选择的最少的个数 |
| `max` | integer | 选择的最多的个数 |
| `player_list` | `list[integer] | null` | 在`object`是`player`时使用，代表可选玩家，若为null则为任意玩家 |

| `object` | 说明 |
| -------- | ---- |
| `hand_card` | 手牌 |
| `card` | 卡牌列表 |
| `player` | 玩家 |
| `hand_maj` | 手中的麻将牌 |

## update_keyword 更新奖励词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `keyword` | string | 奖励词内容 |
| `jibi` | integer | 奖励击毙 |

## update_hidden_keyword 更新隐藏奖励词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `keywords` | `list[string]` | 奖励词内容 |

## 与结算对应的部分

### OnKeyword 接到奖励词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `keyword` | string | 奖励词内容 |
| `jibi` | integer | 奖励击毙 |
| `left` | integer | 今日剩余奖励词击毙数 |

### OnHiddenKeyword 接到隐藏奖励词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `keyword` | string | 隐藏奖励词内容 |

### OnDuplicatedWord 接到重复词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `word` | string | 词内容 |

### OnBombed 接到炸弹

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `word` | string | 词内容 |

### OnDragoned 成功接龙

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `father_node_id` | string | 父节点编号 |
| `node_id` | string | 接龙节点编号 |
| `word` | string | 接龙词 |

### OnStatusAdd 添加状态

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `status` | ProtocolData | 状态，`type: status` |
| `dodge` | bool | 是否闪避 |

### OnStatusRemove 移除状态

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `status` | ProtocolData | 状态，`type: status` |
| `dodge` | bool | 是否闪避 |

### OnJibiChange 击毙变动

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `jibi_change` | integer | 击毙变动值 |
| `current_jibi` | integer | 变动后击毙值 |
| `is_buy` | bool | 是否是购买 |

### OnEventPtChange 活动pt变动

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `eventpt_change` | integer | 活动pt变动值 |
| `current_eventpt` | integer | 变动后活动pt值 |
| `is_buy` | bool | 是否是购买 |

### OnDeath 死亡

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `time` | integer | 死亡时间，单位分钟 |
| `dodge` | bool | 是否闪避 |
| `killer` | int | 杀人者的qq，-1代表无 |

## attacked 被攻击

对应结算有攻击者方和被攻击者方两个。

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `name` | string | 攻击名 |
| `dodge` | bool | 是否闪避 |
| `attacker` | int | 攻击者的qq |

## draw_and_use 抽即用卡牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `cards` | `list[ProtocolData]` | 抽到的卡牌列表，`type: card` |

## use_card 使用卡牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `card` | `ProtocolData` | 使用的卡牌，`type: card` |

## draw_cards 抽取卡牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `cards` | `list[ProtocolData]` | 抽到的卡牌列表，`type: card` |

## discard_cards 弃牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `cards` | `list[ProtocolData]` | 弃置的卡牌列表，`type: card` |

## remove_cards 烧牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `cards` | `list[ProtocolData]` | 移除的卡牌列表，`type: card` |

## get_item 获得物品

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `count` | `integer` | 物品个数 |
| `useable_item` | `ProtocolData` | 物品信息，`type: useable_item` |

## 使用物品

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `count` | `integer` | 物品个数 |
| `useable_item` | `ProtocolData` | 物品信息，`type: useable_item` |

## 获得装备

## 使用装备

## 活动

## 麻将

## begin 结算开始

此项一般不会明显显示（但仍会打包发送出去），表示一项结算开始（push stack），用于更新显示、记录log或是统计数据。

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `name` | string | 结算名称 |

## end 结算结束

此项一般不会明显显示（但仍会打包发送出去），表示一项结算结束（pop stack），仅用于没有相同名字的结算（如BeforeDragoned）。用于更新显示、记录log或是统计数据。如果有failed，一般就没有end了。

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `name` | string | 结算名称 |

## 信息相关

### card 卡牌信息

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | 卡牌id |
| `name` | string | 卡牌名 |
| `description` | string | 卡牌描述 |
| `eff_draw` | bool | 是否是抽到即生效的卡牌 |

### status 状态信息

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | 状态id |
| `name` | string | 状态名 |
| `description` | string | 状态描述 |
| `null` | bool | 是否是可堆叠状态 |
| `count` | integer | 状态层数 |

### equipment 状态信息

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | 装备id |
| `name` | string | 装备名 |
| `description` | string | 装备描述 |

### useable_item 可使用物品信息

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | 状态id |
| `name` | string | 状态名 |
| `description` | string | 状态描述 |

### node 接龙树节点

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | string | 节点id |
| `word` | string | 接龙词 |
| `status` | string | 可选，如下： |

| 不可接龙原因 | 说明 |
| ----- | ---- |
| `too_near` | 距离过近不可接龙 |
| `death` | 死亡不可接龙 |
| `cant_dragon` | 因不可接龙状态不可接龙 |

### quest 任务

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | 任务id |
| `description` | string | 任务描述 |
| `jibi` | integer | 完成奖励击毙数 |
| `count` | integer | 剩余完成次数 |

### sign 星座

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `id` | integer | id |
| `name` | string | 星座名 |
| `description` | string | 星座描述 |

## check 查询结果

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
| profile | 查询自己当前资料。 |
| global_profile | 查询全局资料。 |
| hand_cards | 查询自己当前手牌。 |
| status | 查询自己当前状态。 |
| global_status | 查询当前全局状态。 |
| equipments | 查询自己当前装备。 |
| useable_items | 查询自己当前物品。 |
| quests | 查询自己手牌中的任务之石的任务。 |
| maj | 查询自己的麻将。 |
| jibi | 查询自己的击毙数。 |
| shop | 查询可购买项目。 |
| bingo | 查询bingo活动进度。 |

其他查询结果如下：

### *_pool 池大小

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `pool` | integer | 大小数据 |

### keyword 奖励词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `keyword` | string | 奖励词 |

### active 活动词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `shouwei` | bool | 表示接龙者是否应遵循首尾接龙规则，不考虑诸如`USB Type-C`等的转换。 |
| `weishou` | bool | 表示接龙者是否应遵循尾首接龙规则，不考虑诸如`USB Type-C`等的转换。 |
| `words` | `list[ProtocolData]` | 活动词列表，每个词`type: node` |

### profile 资料

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `jibi_time` | integer | 今日剩余获得击毙次数 |
| `keyword_jibi` | integer | 今日剩余获得关键词击毙 |
| `draw_time` | integer | 抽卡券 |
| `card_limit` | integer | 手牌上限 |
| `shop_card` | integer | 商店抽卡剩余次数 |

### global_profile 全局资料

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `sign` | ProtocolData | 当前星座，`type: sign` |

### hand_cards 手牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `cards` | `list[ProtocolData]` | 卡牌列表，`type: card` |

### *status 状态/全局状态

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `status` | `list[ProtocolData]` | 状态列表，`type: status` |

### equipments 装备

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `equipments` | `list[ProtocolData]` | 装备列表，`type: equipment` |

### useable_items 可使用物品

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `useable_items` | `list[ProtocolData]` | 可使用物品列表，`type: useable_item` |

### quests 任务

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `quests` | `list[ProtocolData]` | 任务列表，`type: quest` |

### maj 麻将

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| TODO | TODO | TODO |

### jibi 击毙

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `jibi` | integer | 当前击毙数 |

### shop 商店

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| TODO | TODO | TODO |

### bingo bingo活动进度

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| TODO | TODO | TODO |
