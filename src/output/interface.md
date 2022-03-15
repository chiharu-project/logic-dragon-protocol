# 输出端接口

输出由代表响应的数据段组成的list构成。此接口兼用于写log，与保存统计数据。

每个数据段都必须包含`type`字段，标明输出类型。对于不同的输出，其余参数如下。

# 结算相关类型



# 可能的输出及包含的数据

## succeed 成功

没有作用。

## failed 失败

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `error_code` | integer | 异常代码，表示失败原因 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算，有时有用 |

| `error_code` | 说明 | 是否需要`settlement` |
| ------------ | ---- | ----------------- |
| 0 | 运行时错误 | 是 |
| 1\*\* | 接龙类 | - |
| 100 | 节点未找到 | 否 |
| 101 | 节点已分叉，并不可再次分叉 | 否 |
| 102 | 节点不是活动节点，且不可分叉 | 否 |
| 110 | 接龙词包含非法字符 | 否 |
| 120 | 未达到接龙间隔 | 是 |
| 121 | 无法接龙 | 是 |
| 190 | 手牌超出上限，不允许接龙 | 否 |
| 2\*\* | 使用类 | - |
| 200 | 未找到卡牌 | 否 |
| 201 | 卡牌不在手牌内 | 否 |
| 202 | 卡牌不满足使用要求 | 是 |
| 210 | 未找到装备 | 否 |
| 211 | 玩家未拥有该装备 | 否 |
| 212 | 装备不满足使用要求 | 是 |
| 219 | 手牌超出上限，不允许使用装备 | 否 |
| 220 | 未找到物品 | 否 |
| 221 | 玩家未拥有该物品 | 否 |
| 222 | 物品不满足使用要求 | 是 |
| 223 | 物品不足，无法使用 | 否 |
| 229 | 手牌超出上限，不允许使用物品 | 否 |
| 3\*\* | 抽卡类 | - |
| 300 | 抽卡券不足 | 否 |
| 390 | 手牌超出上限，不允许抽卡 | 否 |
| 4\*\* | 购买类 | - |
| 400 | 商品未找到 | 否 |
| 410 | 击毙不足 | 是 |
| 411 | 活动pt不足 | 是 |
| 420 | 标记的雷不合法 | 是 |
| 421 | 提交的起始词不合法 | 是 |
| 422 | 提交的奖励词不合法 | 是 |
| 430 | 回溯的节点是根节点 | 否 |
| 431 | 回溯的节点不是活动节点 | 否 |
| 440 | 购买本商品次数用尽 | 是 |
| 441 | 不满足购买本商品的要求 | 是 |
| 450 | 起始词池已空 | 否 |
| 451 | 奖励词池已空 | 否 |
| 452 | 隐藏奖励词池已空 | 否 |
| 5\*\* | 弃牌类 | - |
| 500 | 未找到卡牌 | 否 |
| 501 | 卡牌不在手牌内 | 否 |
| 202 | 卡牌不满足弃牌要求 | 是 |
| 510 | 未在手牌超出上限时使用 | 否 |
| 6\*\* | 分叉 | - |
| 600 | 节点未找到 | 否 |
| 601 | 节点已分叉，并不可再次分叉 | 否 |
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

| `object` | 说明 |
| -------- | ---- |
| `hand_card` | 手牌 |
| `card` | 卡牌列表 |
| `player` | 玩家 |
| `hand_maj` | 手中的麻将牌 |

## keyword 接到奖励词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `keyword` | string | 奖励词内容 |
| `jibi` | integer | 奖励击毙 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |
| `left` | integer | 今日剩余奖励词击毙数 |

## update_keyword 更新奖励词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `keyword` | string | 奖励词内容 |

## hidden_keyword 接到隐藏奖励词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `keyword` | string | 隐藏奖励词内容 |
| `jibi` | integer | 奖励击毙 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |

## update_hidden_keyword 更新隐藏奖励词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `keywords` | `list[string]` | 奖励词内容 |

## duplicate_word 接到重复词

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `word` | string | 词内容 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |

## bomb 接到炸弹

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `word` | string | 词内容 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |

## dragon 成功接龙

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `father_node_id` | string | 父节点编号 |
| `node_id` | string | 接龙节点编号 |
| `word` | string | 接龙词 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |

## status_add 添加状态

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `name` | string | 状态名称 |
| `description` | string | 状态描述 |
| `dodge` | bool | 是否闪避 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |

## status_remove 移除状态

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `name` | string | 状态名称 |
| `description` | string | 状态描述 |
| `dodge` | bool | 是否闪避 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |

## jibi_change 击毙变动

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `jibi_change` | integer | 击毙变动值 |
| `current_jibi` | integer | 变动后击毙值 |
| `is_buy` | bool | 是否是购买 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |

## eventpt_change 活动pt变动

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `eventpt_change` | integer | 活动pt变动值 |
| `current_eventpt` | integer | 变动后活动pt值 |
| `is_buy` | bool | 是否是购买 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |

## death 死亡

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `time` | integer | 死亡时间，单位分钟 |
| `dodge` | bool | 是否闪避 |
| `killer` | int | 杀人者的qq，-1代表无 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |

## attack 攻击

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `name` | string | 攻击名 |
| `dodge` | bool | 是否闪避 |
| `attacker` | int | 杀人者的qq，-1代表无 |
| `attacker_settlement` | `list[ProtocolData]` | 攻击者相关嵌套结算 |
| `defender_settlement` | `list[ProtocolData]` | 被攻击者相关嵌套结算，按顺序晚于攻击者相关结算 |

## 获得物品

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `subject_id` | interger | 物品ID |
| `subject_name` | string | 物品名 |
| `subject_description` | string | 物品描述 |

## 使用物品

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `subject_id` | interger | 物品ID |
| `subject_name` | string | 物品名 |
| `subject_effect` | string | 物品效果 |

## 获得卡牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `card_id` |
| `card_name` |
| `card_description` |
| `consumed_on_draw` | boolean |

## 使用卡牌

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `is_draw_and_use` | boolean |
| `card_id` |
| `card_name` |
| `card_effect` |

## 获得装备

## 使用装备

## 活动

## 麻将

## settlement 其他结算

此项一般不会明显显示（但仍会打包发送出去），用于更新显示、记录log或是统计数据。

| 字段名 | 数据类型 | 说明 |
| ----- | ------- | ---- |
| `event_name` | string | 结算名称 |
| `settlement` | `list[ProtocolData]` | 相关嵌套结算 |


