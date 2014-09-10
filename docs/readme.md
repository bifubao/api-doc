API 说明
=====

示例代码
-------------
   * 源码：https://github.com/bifubao/app_demo/

规则
-------------
   * 比特币、人民币，由外部传入给API时，单位均为基础单位，如: BTC，元。内部系统中， *所有金额均为整数存储* ，比特币、人民币单位分别为： Satoshi, 分.
      * 比特币：1 BTC = 100000000 Satoshi (10^8)
      * 人民币：1 元 = 100 分
   * 系统诸多字段采用int64，所以 *强烈建议使用64bit操作系统* ；32bit操作系统下，需要认真测试int32的溢出问题，会产生严重错误；
   * 请保证客户端时钟同步并保持准确，时间戳最大误差为10分钟，少数接口将限制更为严格

请求
-------------

   * 说明
      * 请求：使用HTTP POST发送请求。
      * 返回：plain/text，格式为 =JSON=

JSON返回示例
-------------



    {
       "error_no":0,
       "error_msg":"",
       "result": xxxxxx,
       "exec_time":"0.0756"
    }


| *字段* | *说明* |
|-|-|
| error_no | 错误码。error_no等于0表示正常，非零均为错误。 |
| error_msg | 错误消息 |
| exec_time | 执行时间 |
| result | 结果存放 |



系统错误码
-------------

系统错误码均以9开头，形如9xxxx均系统错误。error_no=1为内置错误。

| *Code* | *Mesasge* |
|-|-|
| 90000 | 无法通过token id查找对应用户 |
| 90001 | 检测到POST为空，HTTP必须只允许POST方式提交 | 
| 90002 | 无法查找用户，请先登录 |
| 90003 | 模块频度限制设置为空 |
| 90004 | 访问超出模块频度限制 |
| 90010 | 无效的计数器或者空签名 |
| 90011 | 无效的请求时间: 与系统时间误差10分钟以上，允许前后各10分钟误差 |
| 90012 | RSA签名校验失败 |
| 90013 | session rsa公钥为空 |
| 90014 | 计数器校验失败 |
| 90015 | 短信/语言验证码不正确 |
| 90016 | 无可用短信/语言验证码记录，请先请求验证码。连续尝试3次错误依然继续提交也会触发次错误 |
| 90017 | Google auth, 等二次验证码错误 |
| 90018 | Google auth, 无法获取google auth的配置，请检查是否启用 |
| 90019 | Google auth, 已使用过的google auth验证码 |
| 90020 | Google auth, 30秒内尝试错误次数超过3次，请30秒后再次尝试 |
| 90021 | 锁定用户余额失败 |
| 90022 | 解锁用户余额失败 |
| 90023 | 用户余额记录检测失败 |
| 90024 | 用户未绑定GA，未绑定手机，无法付款 | 
| 90025 | 必须绑定手机和邮箱 |
| 90026 | 无法找到对应的APP |
| 90027 | APP计数器校验失败 |
| 90028 | 无法根据app_hash_id载入用户数据 |
| 90029 | 该请求必须携带有效的app_hash_id |
| 90030 | app，该请求为限制路径，无权限 |
| 90031 | 该请求必须来自与web |
| 90032 | 无效的capp_key，无法找到对应的用户 |
| 90033 | capp检查校验值失败 | 
| 90034 | 当前路径不允许capp模式访问 |
| 90035 | 无法找到uniqkey |

全局参数
-------------

请求按照携带 token 和 app_hash_id 区分为两大类：token模式、app模式。

用户登录后可以获取token；app模式通过实名认证后，可以创建app。

### token模式


该方式需要携带token进行通信。有少部分操作，无需登录状态可以执行。

| *名称* | *类型* | *是否必须* | *说明* |
|-|-|-|-|
| ```_time_``` | int | 是 | 时间戳，与服务器时间误差在前后10分钟以内。单位: 秒。 |
| ```_token_``` | string | 否 | token字符串，绝大多数场景需要，部分无需登录接口可以省去 |
| ```_signature_``` | string | 否 | 签名，RSA签名。签名算法： ```SHA512``` |

   * 来自非白名单中IP的请求，当携带_token_时，均需要携带参数：_signature_.  
      * 仅 =/user/login/= 等少数模块，无需携带_token_。

### app模式

该方式使用app_hash_id和app_hash_id对应私钥对数据签名，进行通讯。

| *名称* | *类型* | *是否必须* | *说明* |
|-|-|-|-|
| ```_time_``` | int | 是 | 时间戳，与服务器时间误差在前后10分钟以内。单位: 秒。 |
| ```_app_hash_id_``` | string | 是 | _app_hash_id_ |
| ```_signature_``` | string | 是 | 签名，RSA签名。签名算法： ```SHA512``` |

### 商家API模式

该方式使用 商家编号（pid）和密钥（key） 对应私钥对数据签名，进行通讯。
这种方式的授权获得较为容易，适用于查询记录、创建收款等，但是只能使用一部分Api，不能进行用户操作以及发款等行为。
关于更多支付Api模式的信息，参见页面 [商家API模式](#docs/merchant_api)

| *名称* | *类型* | *是否必须* | *说明* |
|-|-|-|-|
| ```_time_``` | int | 是 | 时间戳，与服务器时间误差在前后10分钟以内。单位: 秒。 |
| ```_pid_``` | string | 是 | 商家编号(pid) |
| ```_checksum_``` | string | 是 | MD5校验和 |


调试
-------------

为调试方便，我们开发了简单的调试帮助页面，以提高开发者效率

   * token模式调试: https://api.bifubao.com/&lt;version&gt;/test/
   * app模式调试: https://api.bifubao.com/&lt;version&gt;/testapp/
   * 商家api模式调试: https://api.bifubao.com/&lt;version&gt;/test/

参数签名规则
-------------

   * 使用数组存入参数，key => value（value无需urlencode()）
   * 按照key =升序= 对数组进行排序
   * 遍历数组，拼接出待签名字符串： =key1 + value1 + key2 + value2 + ... + keyN + valueN=
   * 签名之(RSA, SHA512)，获取签名值

   * PHP打包签名数据示例：

    // generate sign data string
    function bifubao_make_sign_data($arr) {
      unset($arr['_signature_']);
      ksort($arr);
      $sign_str = '';
      if (!empty($arr)) {
        foreach ($arr as $_k => $_v) {
          $sign_str .= $_k . $_v;
        }
      }
      return $sign_str;
    }

    // 示例，当服务端获取请求后，会调用该函数获取待签名数据：
    $to_sign_data = bifubao_make_sign_data($_POST);






回调通知
-------------

回调是将数据以HTTP POST方式发回至用户预留的URL。

   * 币付宝回调签名的RSA公钥


    -----BEGIN PUBLIC KEY-----
    MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAqUSnx8dqJ0UC0jvFTEdL
    gde7BSmKi8GzDnxvu/AMQw7TG3pRKAAKQJRYUSqpgMyOwUSrv3yfu3gBJwufjWJz
    Kgtm8D9TOoYnZMJm4x5Lv9/EpYEg0zrAsmU/6rZJ9mYRaNPrt811Thju0/19fa77
    XnsQ78UmvV4zCePkKAArO70SsU/hf1SinDX//t0a3/UOk0DhKoJZpzjb5mb+dcXM
    GOJKpAONDGDK2UE1W67HmIG72b/R/G8CAFYbw4MGCjb0/Ee6obcAGK3Cj1JcuHjH
    NzymBH0NuDvyz7fJuTg9Eplnh1blNeCJoG/vv7VLZNKetTMTx+H2X534RUQ4XheX
    4QIDAQAB
    -----END PUBLIC KEY-----

请保存至客户端，用于币付宝服务端发起请求时检测签名

   * 回调通知的内容，示例（php var_dump解析为数组结构）：


    // var_dump($_POST)

    array(5) {
      ["_signature_"]=>
      string(344) "TqHbW/7w3osX99XhbGsDaQFEDL6EG3fH3yB1b2pK3xFY+bObDqapjL4bkJDReackGi0ijIt7rHsMBF9tp3ecMPJ5ZjvHjSN+gcdqJP3DXVhsaYf91mzBUJjeKK0y/jlC4uHcppArqjwhPO07THEDVYuJxQHQ5wi9cBauADaJ0JjqJMFu3dQeSqzJwFq5Nsa0JXaelQ6SGw29Gub2hr80b2DEBivftCWCsZqGjPy2lpZuDBv6nKESaqh5vfBMjUrO+Y6AVeT9KeO711cmSPtZqxbqiT/9cg/8tmxbsftMVCn0JYupVDUHw7BHpO/3mvZYMA5JDxlONiXHGUagE2tcKw=="
      ["_request_id_"]=>
      string(1) "4"
      ["_request_check_"]=>
      string(8) "79b502cc"
      ["content"]=>
      string(655) "{"order_id":"147","order_hash_id":"862fccad5a81ef1c4c88304e1b006a474cf5986b3b735d0229fad8c1d380cdb7","external_order_id":"10b4d93aa2051beb71a84020eb69b70749448fe426f559b644b3d10a2101fc1b","external_info":"","display_name":"alabama","display_desc":"..","pay_user_id":"19","price_btc_satoshi":"1000000000","price_cny_fen":"0","pay_btc_satoshi":"1000000000","pay_btc":"10.00000000","ratio_btc2cny_fen":"397398","onchain_receive_btc_address":"1CkkzmS95RLWXZQ1rsR6j15UtzvYoX7xq1","onchain_leave_message":"","offchain_leave_message":"\u62ff\u597d","order_receipt_id":"612543120380","creation_time":"2014-03-06 12:21:31","last_modify_time":"2014-03-07 05:33:12"}"
      ["content_type"]=>
      string(9) "order_pay"
    }


| *字段* | *说明* |
|-|-|
| ```_signature_``` | rsa签名 |
| ```_request_id_``` | 服务端回调ID，全局唯一 |
| ```_request_check_``` | 本次请求的校验码，回调URL应返回该校验码，表示回调接收正常 |
| content | 内容，格式为JSON |
| content_type | 字符串，表示内容类型 |

   * content_type支持类型

| *值* | *说明* |
|-|-|
| order_pay | 订单收到付款通知 |


   * 回调失败处理
      * 失败后重试3次
      * 失败重试间隔时间（秒）：10, 20, 40
      * 请求重放请联系技术支持

API详情
-----------
  - [Api文档v00002](#docs/api_doc_v00002)