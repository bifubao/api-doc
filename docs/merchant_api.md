商家API说明
=====

## 简介
商家API提供了较为简单的接口和较为容易的授权获得方式，使得开发更加容易上手，是我们推荐的API认证方式。

## 如何获取授权
请前往 [商家API获取页面(需要登录)](https://www.bifubao.com/merchants/#api) 获得商家编号(pid)和密钥(key)

## 全局变量
该方式使用 商家编号（pid）和密钥（key） 对应私钥对数据签名，进行通讯。
这种方式的授权获得较为容易，适用于查询记录、创建收款等，但是只能使用一部分Api，不能进行用户操作以及发款等行为。
关于更多支付Api模式的信息，参见页面 [商家API模式](#docs/merchant_api)

| *名称* | *类型* | *是否必须* | *说明* |
|-|-|-|-|
| ```_time_``` | int | 是 | 时间戳，与服务器时间误差在前后10分钟以内。单位: 秒。 |
| ```_pid_``` | string | 是 | 商家编号(pid) |
| ```_checksum_``` | string | 是 | MD5校验和 |

## API请求路径

### 一般情况
请求 https://api.bifubao.com/v00002/&lt;api_path&gt;/

参考 [Api文档v00002](#docs/api_doc_v00002)

### 特例
#### 发起收款
为了使用方便，发起收款采用的是www域下的一个特殊路径<br>
https://www.bifubao.com/merchant-api/order/


商家API一般情况
======
## Api接口中允许使用商家API认证方式的列表
| *api_path* | *说明* | 
|------------|-------------|
| /balance/detail/ | 余额变更详情 |
| /contact/list/ | 联系人列表 |
| /exchange/ | 市场价格 |
| /mobile/version/ | 获取移动端版本号和下载地址 |
| /order/createexternal/ | 创建外部的订单 |
| /order/detail/ | 订单详情 |
| /order/query/ | 订单详情(通过外部ID) |
| /product/detail/ | 商品详情 |
| /product/list/ | 商品列表 |
| /redemption/batchdetail/ | 币券批次详情 |
| /redemption/batchlist/ | 币券批次列表 |
| /redemption/codedetail/ | 查看币券详情 |
| /redemption/redeem/ | 提取币券 |
| /tx/checkaddr/ | 检测收款人地址 |
| /tx/detail/ | 交易详情 |
| /tx/list/ | 交易详情 |
| /tx/unconfirm/ | 交易列表 |
| /user/info/ | 查看当前用户信息 |
| /uniqkey/create/ | 创建唯一键 |

商家API特例
======
## 发起收款

### SDK
[币付宝商家支付SDK (PHP, Python)](https://github.com/bifubao/merchant_api_sdk/archive/master.zip)

### 请求路径：```https://www.bifubao.com/merchant-api/order/```
发起收款的API参数与标准API的 /order/createinternal/ 一致，同时支持GET与POST方式传递变量

### 参数列表
参数列表与标准API创建外部订单 /order/createexternal/ 一致

**名称**       |     **类型**        |          **必须**                      |                 **说明**
--------------------- | --------------------------- | ------------------------------------------------ | ----------------------------------------------------------------
external_order_id     | string, 小于64个字符  |  是                                          |     外部订单ID，每个商户(user_id)下的订单ID必须唯一
external_info         | string , 小于64个字符 |  否                                          |     其他补充信息
price_btc             | number               |        是(price_btc/price_cny必须有一个大于零) | 比特币定价，单位btc。范围：[0.0001, 20000000]
price_cny             | number               |        是(price_btc/price_cny必须有一个大于零) | 人民币定价，单位元。范围：[1, 1000000000]
display_name          | string，小于100个字符 | 是                                           |    显示名称，通常是商品名称等信息
display_desc          | string，小于200个字符 | 否                                           |    显示描述信息，通常是简介，备注等信息
external_callback_url | string，小于128个字符 | 否                                           |    回调通知URL
external_redirect_url | string，小于128个字符 | 否                                           |    重定向至目标URL


### 错误
如果出现错误则在页面内直接展示

### 返回
返回内容为目标付款页面

