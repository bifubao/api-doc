Api文档v00002
=================

用户
====

## 用户注册 /user/register/
### 请求路径： <code>/user/register/</code>
### 参数表


**名称**    |  **类型** | **是否必须** | **说明**
----------------|----------|------|--------------------------------------
is_try_register | int      |  否  |            0 or 1, 1表示尝试注册，会检测字段是否合法有效；不符合则返回响应的错误。（可以只携带部分字段）
email           | string   |  是  |            邮箱
password        | string   |  是  |            密码, 长度：[8，50]
referer         | string   |  否  |            注册推荐源

密码
* 不在已知弱口令字典中

返回信息：成功后返回该用户信息
开启尝试注册时，成功返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.0756"
    }

正式注册返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "user_id":"10",
          "user_email":"test@example.com",
          "user_hash_id":"9455d079fc703ca61b5a42415fd5ac6b",
          "password_hash":"16e03f626fe14cd2fe2aad2c852e032e",
          "mobile_phone_num":"",
          "bitpass_btc_address":"",
          "two_step_google_id":"0",
          "is_email_verified":"0",
          "last_email_send_time":"2014-02-17 11:28:54",
          "default_btc_address":"17D8JkutK8m1915othbkYerEVNKyHb9JaS",
          "timezone":"8",
          "creation_time":"2014-02-17 11:28:54",
          "last_modify_time":"2014-02-17 11:28:54"
       },
       "exec_time":"0.4977"
    }


### 错误码 
#### 注册

**Code** | **Mesasge**
-------- | ----------------------------
10001  |  注册关闭
10002  |  用户名含有特殊字符
10003  |  用户名长度不符合
10004  |  用户名已存在
10005  |  密码长度必须是[6, 50]
10006  |  邮箱格式错误
10007  |  邮箱已存在
10008  |  已知弱口令
10009  |  生成比特币地址失败
10010  |  密码强度过低



## 用户手机注册 /user/regbyphone/
### 请求路径： <code>/user/regbyphone/</code>
通过手机注册的用户，默认Email地址为: <code>13800138000@_bifubao_.com</code> (13800138000为该用户手机号码)

### 参数表

**名称**         | **类型** | **必须** | **说明**
--------------- | -------- | --------| ---------------------------------------------------------------------------------------------------------
is_try_register | int      |  否     |    0 or 1, 1表示尝试注册，会检测所有字段是否合法有效；不符合则返回响应的错误
phone_no        | number   |  是     |    大陆11位手机号码
password        | string   |  是     |    密码, 长度：[6，50]
pin_code        | number   |  是     |    六位验证码。1分钟之内的下发的验证码
referer         | string   |  否     |    注册推荐源


### 错误码 - 注册

**Code** | **Mesasge**
-------- | ---------------------
10020    | 无效的手机号码
10021    | 该号码已经存在



### 返回信息：成功后返回该用户信息
开启尝试注册时，成功返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.0756"
    }



## 用户登录（通过app) /user/loginbyapp/
### 请求路径： <code>/user/loginbyapp/</code>
### 参数表

**名称** | **类型** | **必须**  |  **说明**
----------- | ---------- | ----| --------------------------------------------------------------------------------------
is_need_rsa | int        | 否  | 默认1 是否需要生成rsa公钥私钥(服务端仅保存公钥，客户端请保存私钥)

### 返回与 /user/login/ 一致



## 用户登录 /user/login/
### 请求路径： <code>/user/login/</code>
### 参数表

**名称**    | **类型** | **必须** | **说明**
-------------- | ---------- | ---------- | --------------------------------------------------------------------------------------
phone_or_email | string     | 是         | 用户名或者邮箱
password       | string     | 是         | 密码
ga_code        | string     | 否         | 二次验证码
is_need_rsa    | boolean    | 否         | 是否需要生成rsa公钥私钥(服务端仅保存公钥，客户端请保存私钥)
 

### 登录错误码

**Code**                          | **Mesasge**
--------------------------------- | ---------------------------------------------------------------------------------
11000                             | 用户名或者邮箱为空
11001                             | 密码为空
11002                             | 手机/邮箱与密码不匹配
11003                             | 登录失败，用户名或密码错误
11004                             | 内部错误，更新登录计数器失败
11005                             | 内部错误，生成session失败
<del>11006</del>，由90017代替 <del>| 二次验证码不匹配（此时用户名&密码已经校验通过）</del>
11007                             | 内部错误，保存登录日志失败
11008                             | 邮箱尚未通过验证


### 示例返回（is_need_rsa=1时）
    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "session":{
             "session_id":"3281",
             "user_id":"17",
             "token":"beb23a88430544c33d85e596bc7818ba9720a153548a7eea86a3fa87a4505a69",
             "expire_time":"2014-09-30 05:53:23",
             "counter":"1",
             "rsa_public_key":"-----BEGIN PUBLIC KEY-----
    MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA70KpczmyOjxNlKHHRLEL
    r8aeiAFyORhPq1Mlnbmao5YOUxeneGV7j5JryTQ+J68qTZ8TNRGznN54vkQ1B5BX
    33X4/a2D3Fqokb+V/IcWAoPm3JIVikP4tr+JBbZjzfDmlNM50zhLq0AqQViErMWw
    bPkZBC+baulSippveBcljT13KbqoUd+YXDSfe0p97vBCl6UU5xEw0zEUUebmWBv+
    BWq2m7pMgjU0ZBwT0JA/cwwq1Djt92zFqn5WC/sisyLOd7NBmjQqAc8u9ZoZbZf6
    6DBcuxhcQhHr9dRBHsI82AumSXEwTwWlDicRzEtgY+xgv4DrGAu17+4YMa390LOH
    3wIDAQAB
    -----END PUBLIC KEY-----
    ",
             "creation_time":"2014-09-16 05:53:23",
             "last_modify_time":"2014-09-16 05:53:23",
             "rsa_private_key":"-----BEGIN PRIVATE KEY-----
    MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQDvQqlzObI6PE2U
    ocdEsQuvxp6IAXI5GE+rUyWduZqjlg5TF6d4ZXuPkmvJND4nrypNnxM1EbOc3ni+
    RDUHkFffdfj9rYPcWqiRv5X8hxYCg+bckhWKQ/i2v4kFtmPN8OaU0znTOEurQCpB
    WISsxbBs+RkEL5tq6VKKmm94FyWNPXcpuqhR35hcNJ97Sn3u8EKXpRTnETDTMRRR
    5uZYG/4FarabukyCNTRkHBPQkD9zDCrUOO33bMWqflYL+yKzIs53s0GaNCoBzy71
    mhltl/roMFy7GFxCEev11EEewjzYC6ZJcTBPBaUOJxHMS2Bj7GC/gOsYC7Xv7hgx
    rf3Qs4ffAgMBAAECggEBAJQxJeNIiSuoziSRT2ssfaCR1P9IZgcXF8F17oaSv0Uz
    XAv7Sq83rCBxAHHO+fB6fik1rh/gpu8ynHa8qFvW+5Zc66u0HUgOnvonQC75PJiy
    OfvEP7M0BoiqeaQgJBEadLcZXWeGJtgbFhEDUqnwiCX245fEJO9DNOzEWuZ277ta
    dBrmkbpW5KVQIDtf+BH2gljQa7Qv3LchSGiFoR68Zh4wB+m337h1/QdgydWnzTYX
    PbVgiotuqRJFoFQTs4mF1ofXCle5VnB11qW8CYRmZL99lkxB7D0r1VmHmRnaZpV1
    wx19uKNetK3J7/YXNNbNeW8kFS0GMDjrSe/grS2DOwECgYEA/ThzL96pe0qKcEne
    7U+P1kwnYvghm1SSUmj0U/LsCHET0tj7BRiwrRHsett3gOogRlG0zavf0KUv4X5d
    os+vlXvsuSbHPCuLWsXZIJpn4GPAbsHEi5zAoAQU+giB71YwFtCjhy2MEZTpGlcv
    VSkwSUXOcxspf1cqyiKvkx5F0okCgYEA8eL76alYBkm9Hqw441qRPBDYyQMLU9x5
    1By5mYg3stA9OI/Ka4jdFwdTtuLSfsZyRraYTtakinUegh1H4fy38IwWVhs1vqbc
    wib1Pw2dBmZaMrrqUGYzNemxo4IZJfPnt085fbvqpD8HFm+DtPeEMB4wxsn0fqZQ
    DTTEUCuDjScCgYAiIPadcBRFssE/6yTptTx8tJzmYO0eo2JpSw4LNoWLiboTQ/1p
    LW+6k9zcnjHYJTYeZCrLQawT0f/HE6IJLJbMvfwk5E4cFP5eSKZAF6+Cdc9c3i7g
    ZkssBgDRxk3E9Ufb+1jfAkuLnxdf3npZrwh8B7WQnAuWxyfUQKKLYJwmiQKBgQCj
    Mbk0VISG+YkXAKsA+UGzfMpGFD+4PLAVY6v04epyQfyMBLdVBenkd5ULdsz9q3W+
    L8NirX4lzA7hSlANMCwJpvoK7iC8mGeothAQkma2wqdoQqODyvASF7E517SP3pcf
    Hdxz8CzG0588i9AYcEZHEJdoByllqV+pCUjJrhH7UQKBgD+y/u6H4/UHkO+kouSh
    pwbSlxiscA0qFafwokQp7P7bYa4gklGW2w7yPI/Xcft8C8P0zvXifFqpFV6marUR
    yTwEIs/MiM1DQjD75s94RWoFb+VMz+bYvBAhm9/xlere0WJqcscnN8TnDOSFatv0
    Evrm/SlrGikZBV9msDSYkvae
    -----END PRIVATE KEY-----
    "
          },
          "user_info":{
             "user_id":"17",
             "user_email":"test@example.com",
             "user_hash_id":"fe72624db6755c121a0aefc5f1c8deb2",
             "mobile_phone_num":"18510165105",
             "btc_cur_balance":"959913627301",
             "latest_balance_id":"1438",
             "bitpass_btc_address":"",
             "two_step_google_id":"1869",
             "pay_pin_code_id":"31",
             "user_verify_id":"1",
             "is_email_verified":"1",
             "last_email_send_time":"2014-07-04 08:51:32",
             "default_btc_address":"1JLSrYh4hwfdnj7cHNyxdZShRJ8wnc95TX",
             "timezone":"8",
             "creation_time":"2014-02-22 06:47:45",
             "last_modify_time":"2014-09-15 05:02:28"
          }
       },
       "exec_time":"0.7862"
    }

## 用户登出 /user/logout/
### 请求路径： <code>/user/logout/</code>
### 参数表 （空）

**名称** | **类型** | **说明**
---------- | ---------- | ----------


## 用户发送注册激活邮件 /user/sendactive/
用于发送邮箱注册用户的激活邮件

### 请求路径： <code>/user/sendactive/</code>
### 参数表

**名称** | **类型** | **说明**
---------- | ---------- | ------------
email     | string  |   注册邮箱


### 错误码 - 发送注册激活邮件

**Code** | **Mesasge**
-------- | -----------------------------------------
12001    | 邮箱不存在
12002    | 过于频繁，30秒内只能发送一次
12003    | 邮箱已验证





## 用户激活邮件 /user/emailactive/
### 请求路径： <code>/user/emailactive/</code>
### 参数表


**名称** | **类型** | **说明**
---------- | ---------- | ---------------------
enter_code | string     | 注册邮箱激活码

###  错误码

**Code** | **Mesasge**
-------- | -------------------------------------------------------------
12004    | 无效激活码（可能已经过期，默认有效期1天）
12005    | 该用户已经激活




## 用户绑定邮件 /user/bindemail/
用于手机注册用户绑定邮箱。要求必须登录后操作。

### 请求路径： <code>/user/bindemail/</code>
### 参数表

**名称**    | **类型**   | **必须**   | **说明**
---------- | ---------- | -------- | -----------------------------------------------------------------------------------------------------------------
email      | string     | 是        | 邮箱地址
is_try     | int        | 否        | 若为1只发送激活邮件。若为0，必须携带enter_code，验证通过后，则将email更新至数据库
enter_code | string     | 否        | 进入码


### 错误码

**Code** | **Mesasge**
-------- | --------------------------------------------------------
12006    | 该地址已使用
12007    | 用户已绑定Email（不区分是否已经激活）
12009    | enter_code 错误，请重新发送绑定邮箱的邮件



## 用户绑定手机 /user/bindphone/

绑定手机号码

### 请求路径： <code>/user/bindphone/</code>
### 参数表

**名称** | **类型**      | **说明**
---------- | ---------- | ----------------------
phone_no   | number     | 手机号码
pin_code   | number     | 文字/语音验证码


### 错误码

**Code** | **Mesasge**
-------- | ------------------
12008    | 该号码已使用




## 用户修改密码 /user/changepw/
### 请求路径： <code>/user/changepw/</code>
### 参数表

**名称**     | **类型** | **说明**
------------ | ---------- | ----------
password     | string     | 原密码
new_password | string     | 新密码

### 错误码

**Code** | **Mesasge**
-------- | ---------------------------------
13003    | 密码或新密码为空
13004    | 原密码与新密码一致
13005    | 原密码错误
13006    | 内部错误：更新密码失败



## 用户解绑GA /user/unbindga/

### 请求路径： <code>/user/bindga/</code>
### 参数表


**名称** | **类型** | **必须** | **说明**
---------- | ---------- | ---------- | ------------------------
code      | number, 6 | 否   |     六位数字的验证码


### 错误码

**Code** | **Mesasge**
-------- | --------------
10018    | 尚未绑定GA




## 用户绑定GA /user/bindga/

### 请求路径： <code>/user/bindga/</code>
### 如果参数为空，则返回一个16字符长的验证密钥
### 若参数不为空：
#### 参数表

**名称** | **类型** | **必须** | **说明**
---------- | ---------- | ---------- | -------------------------------------------------------------------------------------------------------------
code      | number, 6  | 否         |  六位数字的验证码。当code为空时，则自动生成一个新的secret码，由前端负责显示。

#### 错误码

**Code** | **Mesasge**
-------- | ------------------------------------------------------------
10014    | 该用户已经绑定GA
10015    | 验证码非法，必须为六位数字
10016    | 验证码验证失败，（请校对终端设置的时间）
10017    | 内部错误：更新数据库失败

#### 返回 - code为空时：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "new_secret":"HYKDBG4JRIJKVVND"
       },
       "exec_time":"0.1453"
    }




## 用户置换token /user/refreshtoken/

提供当前登录用户的密码，置换新的token

### 请求路径： <code>/user/refreshtoken/</code>
### 参数表

**名称** | **类型** | **说明**
---------- | ---------- | ------------
password  | string |    用户密码

### 错误码
### 成功后返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "new_token":"9647cd6a8df76b0b822a1de7b6a74a8c25a139a9817daabc2bb4b31518c76e17"
       },
       "exec_time":"0.2088"
    }




## 用户找回密码 /user/forgotpw/
### 请求路径： <code>/user/forgotpw/</code>
### 参数表

**名称** | **类型** | **必须**                     |                   **说明**
---------- | ---------- | ------------------------------------------------- | ----------------------------------
email      | string     | 是                                  |              注册邮箱
ga_code    | number     | 是（ga_code/pin_code必须有一个不为空） | google二次验证码
pin_code   | number     | 是（ga_code/pin_code必须有一个不为空） | 短信（文字/语音）验证码

### 错误码


## 用户重置为新密码 /user/resetpw/
### 请求路径： <code>/user/resetpw/</code>
### 参数表

**名称**  | **类型** | **必须** | **说明**
------------ | ---------- | ---------- | ---------------------
enter_code   | string     | 是        | 注册邮箱激活码
new_password | string     | 是        | 新密码

### 错误码

**Code** | **Mesasge**
-------- | ----------------------------------------------------------------
12004    | 无效激活码（可能已经过期，默认有效期5分钟）


## 获取当前用户信息 /user/info/
### 请求路径：<code>/user/info/</code>

### 参数表

**名称** | **类型** | **说明**
---------- | ---------- | ------------

### 错误码

**Code** | **Mesasge**
-------- | ----------------------------------------------------------------

### 成功返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "user":{
             "user_id":"17",
             "user_email":"test@example.com",
             "user_hash_id":"fe72624db6755c121a0aefc5f1c8deb2",
             "mobile_phone_num":"18510165105",
             "btc_cur_balance":"959913627301",
             "latest_balance_id":"1438",
             "bitpass_btc_address":"",
             "two_step_google_id":"1869",
             "pay_pin_code_id":"31",
             "user_verify_id":"1",
             "is_email_verified":"1",
             "last_email_send_time":"2014-07-04 08:51:32",
             "default_btc_address":"1JLSrYh4hwfdnj7cHNyxdZShRJ8wnc95TX",
             "timezone":"8",
             "creation_time":"2014-02-22 06:47:45",
             "last_modify_time":"2014-09-15 05:02:28",
             "settings":{
                "user_setting_id":"5",
                "user_id":"17",
                "is_ga_required_on_login":"0",
                "show_badge_financial":"1",
                "creation_time":"2014-07-04 08:29:03",
                "last_modify_time":"2014-09-16 04:00:01"
             }
          },
          "session":{
          }
       },
       "exec_time":"0.0726"
    }



短信/语音验证码 /phone/sendcode/
----------------------------------------
### 请求路径： <code>/phone/sendcode/</code>
### 参数表

**名称**  |  **类型** | **必须** | **说明**
------------- | ---------- | ---------- | ------------------------------------------------------------------------------------------------------------------
phone_no      | number     | 是         | 手机号码
is_voice      | int        | 否         | 1：为语言验证码，0：文字验证码；默认为0。
pin_code_type | int        | 否         | 验证码类型。只有登录用户可以使用该字段。用于鉴别验证码的使用场景，提高安全性


### 错误码

**Code** | **Mesasge**
-------- | ----------------------------------------------------------------
15001    | 无效的手机号码
15002    | 同一个号码，1分钟内只能下发一次。请稍候重试


### 成功返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.0756"
    }





应用
====
## 创建应用 /app/create/

创建一个应用

### 请求路径： <code>/app/create/</code>
### 参数表

**名称** | **类型**       |       **是否必须** | **说明**
---------- | ----------------------- | ------------------ | ----------------------------------------------------------
app_type   | int                     | 是                 | app_类型
label      | string，32字符以内        | 否                 | label, 备注。移动设备，请填写相关设备标识


* app_type说明
  * 100, 限制类APP，主要用于移动等终端，目前仅允许登录等行为, 60天过期
  * 200, 通用类APP，主要用户商家类, 1年过期
  * 201, 通用类APP，主要用户商家类, 5年过期

### 错误码

**Code** | **Mesasge**
-------- | --------------------------------------------------------
19001    | 生成RSA公钥、私钥失败
19002    | 保存app至数据库失败
19003    | 超过最大APP数量，每个用户最多拥有50个APP
19004    | 无效的app_type
19005    | 尚未通过实名认证，请先通过实名认证


### 返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "app":{
             "user_app_id":"2",
             "is_archive":"0",
             "user_id":"31",
             "app_hash_id":"4a05e2fc65a845fb95b540a6bc36d5541afed80bd7a1b551f5196bb6d22f395b",
             "rsa_public_key":"-----BEGIN PUBLIC KEY-----
    MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAsmF5+8G+t12qyKODJUwK
    I2FY9Hfog5Wxel/w+xjC+QsvOVdkSRnOvEQ/j2CSi6vRbbFSjsT+/uXLtG54WYsF
    aVVT7FQY3CJO3eJeqplGIFAfLRlyHxrQzFx9H2LoBPtFlFPGN8VkojXvulfNUSga
    XJ+2GicYJ0x3ICuxKbmdtM3GA9euURMKMSfetJZ2UnuXwS9G6BGBHTuGrdlMuYP9
    jelJ96c77TyBdApZJ8Os5MYQjt52B5mb+cwtdc2UqpW64akgvnkyiurMLbvf2WNx
    RIa1KUT0eECMltgU60l7O5LQ0T85UXEYE1Fpq6SyKtU4MnfiZjAKNyhUliW8P9L6
    owIDAQAB
    -----END PUBLIC KEY-----
    ",
             "counter":"1",
             "expire_time":"2019-03-04 12:24:48",
             "creation_time":"2014-03-05 12:24:48",
             "last_modify_time":"2014-03-05 12:24:48"
          },
          "private_key":"-----BEGIN PRIVATE KEY-----
    MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQCyYXn7wb63XarI
    o4MlTAojYVj0d+iDlbF6X/D7GML5Cy85V2RJGc68RD+PYJKLq9FtsVKOxP7+5cu0
    bnhZiwVpVVPsVBjcIk7d4l6qmUYgUB8tGXIfGtDMXH0fYugE+0WUU8Y3xWSiNe+6
    V81RKBpcn7YaJxgnTHcgK7EpuZ20zcYD165REwoxJ960lnZSe5fBL0boEYEdO4at
    2Uy5g/2N6Un3pzvtPIF0Clknw6zkxhCO3nYHmZv5zC11zZSqlbrhqSC+eTKK6swt
    u9/ZY3FEhrUpRPR4QIyW2BTrSXs7ktDRPzlRcRgTUWmrpLIq1Tgyd+JmMAo3KFSW
    Jbw/0vqjAgMBAAECggEAEca7BEa7fcR813/L+vSH8hyqY7prVmmdhSd4eV1vWKgv
    rZQy70H+Iy7d8qjaEccumLLaGlYxXy+BTFrR7YJ4KJYTRfLfg1h76Yq8il256lBx
    uunVQJpIgoUZBv6xRoXP2kc68qXeMHgVisljMZpAfkiAOjz5IDlziaYxAop31+Ob
    C6KMbyiKZ5hfNVkqIB8v9Y/kOIGzftdtjG4Q093AWYktRZ2RsLJcF8dGCjCq/fyI
    YTn5HUtKpmdMqiZe3VRQlolM98a3TQCStx88Vqq0Lr/VYmwdOB5oH5KdfXX+ZNma
    cceMWTsz+PyEgLGllSJXK0Wc8D0OrkQrRo4GS3I6UQKBgQDlksq0A4M3C5iglf2s
    zSsoIpVjcoY3nv8BMaZZuQ0iRB9eBH3YhXZuswCwZW9UCFcNrUgBYgT0Bg2B5voe
    x+bi0uDiiA+UnOCr+9I2v57cMowGtL4rqbfQC5RSiO8bxpMmXlxlc+oRBa0PsLzp
    vUx8JvfSzdquXW2AywLvL6bYmQKBgQDG6hvoywjYWsx6JGIqkB7ZCCqTXNpFMwTi
    d7TXGIJLObx8Ampxx85aKrNGUMkIDIw9BVJ5OPKxhz9kg5012SaioxxNYQhnVqm/
    7/vyo6nlBAYzSTuT/5XHo0OPlL4E4JiulraHekvo0w+74zlhswjcH7IRo59d/mKw
    kMxMM7RGmwKBgDnsx+iT2k/RRTl/nvoy6mi+ESN+ig8Otxj+BhMtdfrnZWK7j2VK
    h4926v2XGngBgrWYu0peCRHpVQ8p0IJjvgYNX2DJI+VDkAzVBT17LAIzXtFyWWl6
    8T41Lb+FfY9sk0RjGr0eejjBTeFfnsr7UIki6/Tsq/jC6hZNIvhm9ZpBAoGBAJyC
    q/8ZchY6K1WXtx3iVENUZ5uXna6BHEDNC5+LC0oBXcr9Y5+vJTBRFMoo2mTY6qdA
    vsnfAyaoUjrWTCaIfBvP138S8DfPOrVpMIaUPCOUbQSBhL1IhyOT1J7u3CyeQ8Rr
    lac3lO7W0zR07ztuUXRSBBxY8BZXCHQBGp6CuEAtAoGBALwr53uWhRQYF5nmcdyb
    9F6AtwzNGO1UI/Hm2ICG/4thN7ElrvxvdcyGnUF1aEicv+g816Z2AseMzj8A47D6
    zV2u3oTt5hgWQvP1xvQeocNESI5NAeFi3w6LPU2v2U28uLktv7wfjBrpflvwWhUQ
    R49chKOHEEZ4RmDZr2ocacFY
    -----END PRIVATE KEY-----
    "
       },
       "exec_time":"0.4484"
    }



## 应用存档 /app/archive/

存档一个应用，存档后不可还原

### 请求路径： <code>/app/archive/</code>
### 参数表

**名称** | **类型** | **是否必须** | **说明**
----------- | ---------- | ------------------ | -----------
user_app_id | int        | 是                 | user_app_id




## 应用列表 /app/list/

应用列表

### 请求路径： <code>/app/list/</code>
### 参数表

**名称** | **类型** | **是否必须** | **说明**
---------- | ---------- | ------------------ | ---------------------------------------------------------------
app_type   | string     | 是                 | app类型，多个用逗号隔开，例如 <code>100,200</code>
is_archive | int        | 否                 | 是否显示archive的app


### 返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "0":{
             "user_app_id":"3",
             "app_type":"200",
             "is_archive":"0",
             "user_id":"31",
             "app_hash_id":"b43282d0d93373e299bc13c847b2560d8a22b4438dca37ffd4881621b0b99c0f",
             "label":"",
             "rsa_public_key":"-----BEGIN PUBLIC KEY-----
    MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA7p8+vykHtWnENwUeWWmq
    hosCfvMz2SVGREBWt7w0R9Mr+saxMsUp8HREdans5qZvc3pIyGi0LRAeuxOEy9dk
    ju7tYaVFuMLN8jXB5/NNWvJuD7U0INZ2eGG1DqrEEpTKHRkGR4Lyfcay/w+ue7mA
    nurN1Zt7wY1Ab4Lg+wy1ML9UMqrKEC7QLY+91AkaoxRqsDsgml4LsLSCLLlAFneV
    Gty9XwDwko/ilv57VuD5OMLKQQyMLjgP/pwkT/3waJE/Ry20LSfzW/VSgskXr4K7
    Z7lLIkz3MTmABx/jNYsy+79Zyb7EmHIJ6gCvpnCFSiLwhc3GqbyO8RgELHuLxKm9
    DwIDAQAB
    -----END PUBLIC KEY-----
    ",
             "counter":"1",
             "expire_time":"2014-05-05 06:20:49",
             "creation_time":"2014-03-06 06:20:49",
             "last_modify_time":"2014-03-06 06:20:49"
          },
          "1":{
             "user_app_id":"2",
             "app_type":"100",
             "is_archive":"0",
             "user_id":"31",
             "app_hash_id":"4a05e2fc65a845fb95b540a6bc36d5541afed80bd7a1b551f5196bb6d22f395b",
             "label":"",
             "rsa_public_key":"-----BEGIN PUBLIC KEY-----
    MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAsmF5+8G+t12qyKODJUwK
    I2FY9Hfog5Wxel/w+xjC+QsvOVdkSRnOvEQ/j2CSi6vRbbFSjsT+/uXLtG54WYsF
    aVVT7FQY3CJO3eJeqplGIFAfLRlyHxrQzFx9H2LoBPtFlFPGN8VkojXvulfNUSga
    XJ+2GicYJ0x3ICuxKbmdtM3GA9euURMKMSfetJZ2UnuXwS9G6BGBHTuGrdlMuYP9
    jelJ96c77TyBdApZJ8Os5MYQjt52B5mb+cwtdc2UqpW64akgvnkyiurMLbvf2WNx
    RIa1KUT0eECMltgU60l7O5LQ0T85UXEYE1Fpq6SyKtU4MnfiZjAKNyhUliW8P9L6
    owIDAQAB
    -----END PUBLIC KEY-----
    ",
             "counter":"1",
             "expire_time":"2019-03-04 12:24:48",
             "creation_time":"2014-03-05 12:24:48",
             "last_modify_time":"2014-03-05 12:24:48"
          },
          "2":{
             "user_app_id":"1",
             "app_type":"100",
             "is_archive":"0",
             "user_id":"31",
             "app_hash_id":"c553a679994fb4f0f95621e50c25f5749d7d5108bd4812982bbddc57b0b89d91",
             "label":"",
             "rsa_public_key":"-----BEGIN PUBLIC KEY-----
    MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAxFk1oNOgIa0kgmpbFHxp
    znt/xzVNayHbTxZd90d6EYfTMXFbadrzf4slTAC2Plj37zhvtM1sDf9RRwSjRQE7
    2nQV0GMhuOQTZn0CEXdqH4XtJ7NSaV+Hvm6hkTcWv+FuCY770F2gthhSRlR8/BPc
    bONsN1o1gF1pzRUuYXdp07W0AqFbZ3KwLdZiakybAxLJkMvhQALhQqhDHRYtgbDZ
    FsPVRoPHaP6NcN64d4hBwRiVDi5l9lMwUhFGQ65blkj4eqy06VI4hkTj7LduGATI
    V++gSuphprWLvzNt6b4M5Ng6aZECb50mK7+mlCy7q/bnekfQsfTOnJ14iRHuaspZ
    hwIDAQAB
    -----END PUBLIC KEY-----
    ",
             "counter":"1",
             "expire_time":"2019-03-04 12:24:09",
             "creation_time":"2014-03-05 12:24:09",
             "last_modify_time":"2014-03-05 12:24:09"
          }
       },
       "exec_time":"0.2586"
    }



交易
=====

## 发款请求 /tx/send/
该发款并不是立即扣减，而是推入队列，由后续作业执行扣款。大约会有数秒延迟。

### 请求路径： <code>/tx/send/</code>
含义：发款。调用之前建议调用 <code>/tx/checkaddr/</code> 展示更多信息，帮助确认是否有误
### 参数表

**名称**   |  **类型** | **是否必须**                          |                         **说明**
-------------- | ---------- | -------------------------------------------------- | ------------------------------------------------------------------------------------------------
target_address | string     | 是                                                 |  可以是：比特币地址；邮箱；手机号码
btc_amount     | number     | 是                                                 |  转账金额，单位btc。range: [0.0001, 20000000]
ga_code        | number     | 是（ga_code/pin_code必须有一个不为空，web端）         |  google二次验证码
pin_code       | number     | 是（ga_code/pin_code必须有一个不为空，web端）         |  短信（文字/语音）验证码
pay_pin_code   | number     | 是（移动端不填写ga_code/pin_code，须填写pay_pin_code） | 移动端支付PIN码
tx_fee         | number     | 否                                                 |  若交易为向外部发送比特币，则必须填写。目前允许区间：[0.0001, 0.01] BTC
leave_message  | string     | 否                                                 |  50汉字或英文以内


### 错误码

**Code** | **Mesasge**
-------- | ------------------------------------------------------------------------------------------------
16001    | 无效的接收地址
16002    | 无效的金额
16003    | 无效的比特币地址
16004    | 不属于任何用户的内部比特币地址
16005    | 无法发款给自己
16006    | 无效的地址（非比特币地址，即邮箱或手机号等，但又不是币付宝用户）
16007    | 余额不足
16008    | 无效的矿工费(仅限外部发款)。

### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result": {"btc_transfer_request_id": "xxxxxxxxxx"},
       "exec_time":"0.1914"
    }



## 付款至订单 /tx/payorder/
付款至订单

### 请求路径： <code>/tx/payorder/</code>
### 参数表

**名称**  |  **类型** | **是否必须**                      |                             **说明**
------------- | ---------- | ------------------------------------------------------------------- | ----------------------------------
order_hash_id | string     | 是                                                                  | 订单hash id
ga_code       | number     | 是（ga_code/pin_code必须有一个不为空，web端）                          |  google二次验证码
pin_code      | number     | 是（ga_code/pin_code必须有一个不为空，web端）                          |  短信（文字/语音）验证码
pay_pin_code  | number     | 是（移动端不填写ga_code/pin_code，须填写pay_pin_code）                 |  移动端支付PIN码
leave_message | string     | 否                                                                  | 50汉字或英文以内

### 错误码

**Code** | **Mesasge**
-------- | ------------------------------------------------
18012    | 订单不存在，或者已过期、或已支付
18013    | 订单商家不存在
18014    | 订单所关联的商品已下线



## 检测收款人地址 /tx/checkaddr/
检测用户地址

### 请求路径： <code>/tx/checkaddr/</code>
* 含义：发款

### 参数表

**名称**  |   **类型** | **说明**
-------------- | ---------- | ---------------------------------------------------
target_address | string     | 可以是：比特币地址；邮箱；手机号码

### 返回格式：

    {
        "error_no": 0,
        "error_msg": "",
        "result": {
            "target_address": "15vGw1TKZcsxkziQZ4jDFqq6oAxeDaTpVM",
            "address_type": 3,
            "is_internal": 0,
            "target_user": null,
            "is_self": 0
        },
        "exec_time": "0.1763"
    }

    {
        "error_no": 0,
        "error_msg": "",
        "result": {
            "target_address": "wanghb@dearcoin.com",
            "address_type": 2,
            "is_internal": 1,
            "target_user": {
                "user_id": "6",
                "user_email": "wanghb@dearcoin.com",
                "mobile_phone_num": "156*****6852"
            },
            "is_self": 1
        },
        "exec_time": "0.1395"
    }


### 错误码

**Code** | **Mesasge**
-------- | --------------------
16003    | 公用16003错误码




## 获取未完全确认的充值交易 /tx/unconfirm/
获取未完全确认的充值交易

### 请求路径： <code>/tx/unconfirm/</code>
### 参数表

**名称** | **类型** | **是否必须** | **说明**
---------- | ---------- | ------------------ | ----------

### 返回格式：

    array(4) {
      ["error_no"]=>
      int(0)
      ["error_msg"]=>
      string(0) ""
      ["result"]=>
      array(1) {
        [0]=>
        array(14) {
          ["user_address_id"]=>
          string(3) "305"
          ["is_archive"]=>
          string(1) "0"
          ["btc_address"]=>
          string(34) "14qQiG7rV6XVZAQMqGJgjfWwqHNoQUWD2h"
          ["user_id"]=>
          string(1) "5"
          ["label"]=>
          string(0) ""
          ["btc_tx_in_vout_id"]=>
          string(2) "10"
          ["handle_status"]=>
          string(4) "1000"
          ["tx_hash"]=>
          string(64) "c1865154d85218b032fc93c71ecacc6621f99a42a9f7237f20820d3d3e9023c5"
          ["block_hash"]=>
          string(64) "0000000000000000ee584dae0dcbe6c87da572a9af201b1670645531f74d4468"
          ["vout_amount"]=>
          string(6) "646387"
          ["confirmations"]=>
          string(1) "1"
          ["at_least_confirmations"]=>
          string(1) "1"
          ["creation_time"]=>
          string(19) "2014-03-05 06:17:11"
          ["last_modify_time"]=>
          string(19) "2014-03-05 06:20:49"
        }
      }
      ["exec_time"]=>
      string(6) "0.0587"
    }


## 交易列表 /tx/list/

交易列表

### 请求路径： <code>/tx/list/</code>
### 参数表

**名称**                |   **类型** | **必须**   | **说明**
---------------------- | ---------- | ---------- | --------------------------------------------------------------------------------------------------------------------------
page_no                | int        | 是         | 默认：1，分页码
page_size              | int        | 否         | 默认：500。分页大小，范围：[2, 500]
tx_type                | string     | 否         | 交易类型，为空提取所有交易, enom('external_in', 'external_out', 'internal_out', 'internal_in')
user_id_or_btc_address | string     | 否         | 限制交易为某个用户的或者某个地址的。（若为用户tx_type=internal_*  若为地址tx_type=external_*）

### 返回格式：

    {
        "error_no": 0,
        "error_msg": "",
        "result": {
            "total_count": 15,
            "page_no": 1,
            "page_size": 20,
            "items": [
                {
                    "balance_id": "142",
                    "prev_balance_id": "140",
                    "user_id": "6",
                    "event_date": "2014-02-27",
                    "change_event_type": "200",
                    "change_event_id": "141",
                    "diff_balance": "-100011000",
                    "prev_balance": "23643146669",
                    "cur_balance": "23543135669",
                    "related_user_id": "0",
                    "related_btc_address": "",
                    "creation_time": "2014-02-27 07:58:28",
                    "related_event": {
                        "btc_internal_transfer_id": "141",
                        "user_id": "6",
                        "target_user_id": "15",
                        "leave_message": "say hello to bitcoin",
                        "order_id": null,
                        "order_hash_id": null,
                        "order_display_name": null,
                        "order_display_desc": null,
                        "order_price_btc": null,
                        "order_ratio_btc2cny": null,
                        "order_offchain_leave_message": null,
                        "order_seller_hint": null,
                        "order_product_id": null,
                        "order_receipt_id": null,
                        "order_onchain_btc_deposit_id": null,
                        "order_offchain_btc_transfer_request_id": null
                    },
                    "related_user": {
                        "user_id": "15",
                        "user_email": "test@example.com",
                        "mobile_phone_num": "",
                        "contact": {
                            "user_contact_id": "82",
                            "contact_user_id": "15",
                            "label": "",
                            "transfer_counter": "0"
                        }
                    }
                },
                {
                    "balance_id": "139",
                    "prev_balance_id": "137",
                    "user_id": "6",
                    "event_date": "2014-02-27",
                    "change_event_type": "400",
                    "change_event_id": "3",
                    "diff_balance": "-100111000",
                    "prev_balance": "23843268669",
                    "cur_balance": "23743157669",
                    "related_user_id": "0",
                    "related_btc_address": "",
                    "creation_time": "2014-02-27 04:35:19",
                    "related_event": {
                        "btc_withdrawal_id": "3",
                        "tx_hash": "-89",
                        "receive_btc_address": "1hA3TezJkhEA6XvHcjSTGxi8i9n1mcguV",
                        "handle_status": "100"
                    },
                    "tx_hash": "-89",
                    "receive_btc_address": {
                        "btc_address": "1hA3TezJkhEA6XvHcjSTGxi8i9n1mcguV",
                        "contact": {
                            "user_contact_id": "76",
                            "btc_address": "1hA3TezJkhEA6XvHcjSTGxi8i9n1mcguV",
                            "label": "",
                            "transfer_counter": "0"
                        }
                    },
                    "handle_status": "100"
                }
            ]
        }
    }



## 交易详情 /tx/detail/

转账详情

### 请求路径： <code>/tx/detail/</code>
### 参数表

**名称**       |       **类型** | **必须** | **说明**
----------------------- | ---------- | ---------- | ----------
btc_transfer_request_id | int        | 是         | 转账ID

### 返回格式：

市场
=======

## 价格 /exchange/
<del>当价格有效时（两分钟之内有更新），返回价格信息，目前支持火币网。</del>

币付宝会实时（5~10秒）获取五家有良好信誉的中文交易所（<a target="_blank" href="https://www.huobi.com">huobi.com</a> / <a target="_blank" href="https://btcchina.com">btcchina.com</a> / <a target="_blank" href="https://www.okcoin.com">okcoin.com</a> / <a target="_blank" href="https://btctrade.com">btctrade.com</a> / <a target="_blank" href="https://peatio.com">peatio.com</a>）的最新交易价格，并去掉其中最高的价格和最低的价格，平均计算得来。

### 请求路径： <code>/exchange/</code>
### 返回格式：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "price":"292130",
          "time":"2014-09-15 06:26:25",
          "updated_at":"2014-09-15 06:26:25",
          "huobi_price":"292130", // deprecated
          "huobi_last_modify_time":"2014-09-15 06:26:25" // deprecated
       },
       "exec_time":"0.0057"
    }


联系人
========


## 新增联系人 /contact/create/
新增联系人，联系人地址可以是：邮箱、手机号、比特币地址。其中邮箱、手机号均会识别为内部用户;

比特币地址若为内部用户联系，则联系人也识别为内部用户。非本系统的比特币地址则自动识别为外部用户。

联系人表中仅保存用户ID，所以联系人修改相关信息，不会影响本人通讯录的信息显示。

### 请求路径： <code>/contact/create/</code>
### 参数表

**名称**        |   **类型** | **必须** | **说明**
--------------- | ---------- | ---------- | ------------------------------------------------------------------
contact_address | string     | 是         | 联系人地址，可以是：邮箱、手机号、比特币地址
label           | string     | 否         | 备注

### 错误码

**Code**         | **Mesasge**
---------------- | ---------------------------------------------
17001            | 无效的地址
17002            | 保存联系人失败
17003            | 属于联系人自己的地址，不予通过
<del>17004</del> | <del>联系人或地址已经存在</del>

### 返回格式：
通过邮箱添加，成功后返回 - 不会显示手机号码：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "user_contact_id":"13",
          "user_id":"13",
          "contact_type":"2",
          "contact_user_id":"1",
          "label":"",
          "creation_time":"2014-02-21 06:30:58",
          "last_modify_time":"2014-02-21 06:30:58",
          "contact_user":{
             "user_email":"test@example.com"
          }
       },
       "exec_time":"0.1267"
    }


通过手机添加，成功后返回 - 多出邮箱字段：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "user_contact_id":"16",
          "user_id":"13",
          "contact_type":"1",
          "contact_user_id":"2",
          "label":"",
          "creation_time":"2014-02-21 06:32:52",
          "last_modify_time":"2014-02-21 06:32:52",
          "contact_user":{
             "mobile_phone_num":"13911994671",
             "user_email":"test@example.com"
          }
       },
       "exec_time":"0.1796"
    }



## 联系人列表 /contact/list/

### 请求路径： <code>/contact/list/</code>
### 参数表

**名称**      |  **类型**                                   |    **必须** | **默认值**   |              **说明**
------------ | ------------------------------------------- | ---------- | ------------------------------------------ | ---------------------------------------------------------------
page_no      | int                                         | 是         |  1                           |             分页码
page_size    | int                                         | 否         | 500                           |             分页大小，范围：[2, 500]
contact_type | string: enom('contacts', 'btc_address', '') | 否         | 默认为空；为空时返回所有类型     | 通讯录类型：联系人（contacts），比特币地址(btc_address)
is_history   | int                                         | 否         |  0                            |            0：提取手动添加的新联人，1：提取自动添加的联系人，-1：提取所有联系人


### 返回格式：
    {
        "error_no": 0,
        "error_msg": "",
        "result": {
            "total_count": 3,
            "items": [
                {
                    "user_contact_id": "34",
                    "user_id": "17",
                    "contact_type": "1",
                    "contact_user_id": "13",
                    "btc_address": "-1393058011",
                    "label": "",
                    "transfer_counter": "0",
                    "creation_time": "2014-02-22 08:33:31",
                    "last_modify_time": "2014-02-22 08:33:31",
                    "mobile_phone_num": "18600000000"
                },
                {
                    "user_contact_id": "29",
                    "user_id": "17",
                    "contact_type": "1",
                    "contact_user_id": "17",
                    "btc_address": "-1393055253",
                    "label": "",
                    "transfer_counter": "0",
                    "creation_time": "2014-02-22 07:47:33",
                    "last_modify_time": "2014-02-22 07:47:33",
                    "user_email": "test@example.com"
                }
            ]
        },
        "exec_time": "0.0900"
    }



    {
        "error_no": 0,
        "error_msg": "",
        "result": {
            "total_count": 1,
            "items": [
                {
                    "user_contact_id": "31",
                    "user_id": "17",
                    "contact_type": "3",
                    "contact_user_id": "-1393055903",
                    "btc_address": "1JfvCK3NAaKdQZCmu4vBjGhqAzxYrePsHg",
                    "label": "",
                    "transfer_counter": "0",
                    "creation_time": "2014-02-22 07:58:23",
                    "last_modify_time": "2014-02-22 07:58:23"
                }
            ]
        },
        "exec_time": "0.1128"
    }


## 联系人删除 /contact/delete/
### 请求路径： <code>/contact/delete/</code>
### 参数表

**名称**         |   **类型** | **必须** | **默认值** | **说明**
--------------- | ---------- | ---------- | ------------- | ----------
user_contact_id | int        | 是          | 无       |



## 联系人修改 /contact/update/
### 请求路径： <code>/contact/update/</code>
### 参数表

**名称**      **类型** | **必须** | **默认值** | **说明**
--------------- | ---------- | ---------- | ------------- 
user_contact_id | int        | 是          | 无           
label           | string     | 是          | 无            


地址
======

## 新增地址 /address/create/
### 请求路径： <code>/address/create/</code>
### 参数表

**名称** | **类型** | **必须** | **默认值** | **说明**
---------- | ---------- | ---------- | ------------- | ----------
label     | string   |  否           | 地址备注        |


### 错误码

**Code** | **Message**
-------- | ------------------------------------------------
10009    复用10009
10011    当前已超过最大地址数。目前为100。

### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "user_address_id":"75",
          "is_archive":"0",
          "btc_address":"1G1JPwiWfxu2FbHdt65AmoMcF2W2Kw8enS",
          "user_id":"13",
          "label":"",
          "creation_time":"2014-02-26 05:47:20"
       },
       "exec_time":"0.6231"
    }





## 地址列表 /address/list/
### 请求路径： <code>/address/list/</code>
### 参数表

**名称** | **类型** | **必须** | **默认值** | **说明**
---------- | ---------- | ---------- | ------------- | --------------------------------
is_archive | int        | 是         |  0            | 地址是否存档
page_no    | int        | 是         |  1            | 分页码
page_size  | int        | 否         | 500           |  分页大小，范围：[2, 500]

### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "total_count":6,
          "page_no":1,
          "page_size":500,
          "items":{
             "0":{
                "user_address_id":"75",
                "is_archive":"1",
                "btc_address":"1G1JPwiWfxu2FbHdt65AmoMcF2W2Kw8enS",
                "user_id":"13",
                "label":"",
                "creation_time":"2014-02-26 05:47:20"
             },
             "1":{
                "user_address_id":"74",
                "is_archive":"1",
                "btc_address":"1HawqoSBW7E2aYwyB9MjTdGfrGNdCmMwBs",
                "user_id":"13",
                "label":"",
                "creation_time":"2014-02-26 05:46:57"
             },
             "2":{
                "user_address_id":"73",
                "is_archive":"1",
                "btc_address":"1FztALsuCArGtKUXzXvzWBr2agJmocgbAg",
                "user_id":"13",
                "label":"",
                "creation_time":"2014-02-26 05:45:54"
             },
             "3":{
                "user_address_id":"72",
                "is_archive":"1",
                "btc_address":"15YoMUJtv7zJMFBCzaX2JwQ2WngHyFKtAs",
                "user_id":"13",
                "label":"",
                "creation_time":"2014-02-26 05:42:50"
             },
             "4":{
                "user_address_id":"58",
                "is_archive":"1",
                "btc_address":"14tDx98LRxeuyXGB2H1U6C4dPThP9Td1mD",
                "user_id":"13",
                "label":"",
                "creation_time":"2014-02-21 06:03:46"
             },
             "5":{
                "user_address_id":"11",
                "is_archive":"1",
                "btc_address":"1BpcfjDuZD6yNbh2LAwGWE3385Pa6GFoLX",
                "user_id":"13",
                "label":"",
                "creation_time":"2014-02-18 06:15:58"
             }
          }
       },
       "exec_time":"0.1778"
    }



## 存档地址 /address/archive/

存档地址

### 请求路径： <code>/address/archive/</code>
### 参数表

**名称**     |    **类型** | **必须** | **默认值** | **说明**
------------------ | ---------- | ---------- | ------------- | ----------------------------------------------------------------------------
user_address_or_id | string    | 是       | 无   |        用户关联地址的ID(user_address_id) 或者 比特币地址(btc_address)

### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.3002"
    }




## 从存档恢复地址 /address/unarchive/

从存档恢复

### 请求路径： <code>/address/unarchive/</code>
### 参数表

**名称**     |    **类型** | **必须** | **默认值** | **说明**
------------------ | ---------- | ---------- | ------------- | ----------------------------------------------------------------------------
user_address_or_id |  string   |  是    |    无     |      用户关联地址的ID(user_address_id) 或者 比特币地址(btc_address)


### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.3002"
    }




## 更新地址 /address/update/
### 请求路径： <code>/address/update/</code>
### 参数表

**名称**     |    **类型** | **必须** | **默认值** | **说明**
------------------ | ---------- | ---------- | ------------- | ---
user_address_or_id | string     | 是         | 无             | 用户关联地址的ID(user_address_id) 或者 比特币地址(btc_address)
label              | string     | 是         | 无             | 备注

### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.3002"
    }


## 设置用户默认地址 /address/setdefault/
### 请求路径： <code>/address/setdefault/</code>
### 参数表

**名称**     |    **类型** | **必须** | **默认值** | **说明**
------------------ | ---------- | ---------- | ------------- | ---
user_btc_address | string     | 是         | 无             | 比特币地址(btc_address)

### 错误码

**Code** | **Message**
-------- | ------------------------------------------------
10012    | 错误的地址（可能地址不属于该用户或者该地址有误）

### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.3002"
    }



订单
=======

## 创建内部商品的订单 /order/createinternal/
创建内部商品的订单
### 请求路径： <code>/order/createinternal/</code>

### 参数表

**名称**    |  **类型** | **必须** | **说明**
--------------- | ---------- | ---------- | -------------
product_hash_id | string     | 是         | 商品Hash ID

### 错误码

**Code** | **Mesasge**
-------- | ---------------------------------
18006    | 创建收款比特币地址失败
18007    | 保存订单至数据库失败
18010    | 计算所需支付比特币失败
18011    | 获取市场汇率价格失败

### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "order_id":"1",
          "order_hash_id":"ce60f638204ba900314a3af8c2474b299fcea7beb61f5cb1f930b2e785310a26",
          "handle_status":"100",
          "user_id":"13",
          "external_order_id":"7d080f1496fc011373a6b19c5a9f73affe2a4ec1ba14aefc563f95198ade3fe1",
          "external_info":"",
          "product_id":"6",
          "display_name":"参数值需要urlencode()",
          "display_desc":"",
          "price_btc":"0",
          "price_cny":"100000",
          "pay_user_id":"-1",
          "pay_btc":"-1",
          "ratio_btc2cny":"-1",
          "onchain_receive_btc_address":"14ntNhi3gzHKXSp45DyhgkBtsr5eG7WdHg",
          "onchain_leave_message":"",
          "onchain_notify_phone":"",
          "onchain_notify_email":"",
          "onchain_btc_deposit_id":"-1",
          "offchain_btc_internal_transfer_id":"-1",
          "creation_time":"2014-02-28 08:17:45",
          "last_modify_time":"2014-02-28 08:17:45"
       },
       "exec_time":"0.7348"
    }



## 创建外部的订单 /order/createexternal/
创建外部的订单
### 请求路径： <code>/order/createexternal/</code>
### 参数表

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


### 错误码

**Code** | **Mesasge**
-------- | ---------------------------------------------------------------
18001    | 无效的比特币定价
18002    | 无效的人民币定价
18003    | 多个定价均为零
18006    | 创建收款比特币地址失败
18007    | 保存订单至数据库失败
18010    | 计算所需支付比特币失败
18011    | 获取市场汇率价格失败
18015    | 比特币、人民币价格同时出现，仅允许一个出现
18016    | 外部订单ID为空
18017    | 显示名称为空
18018    | 外部订单ID(external_order_id)已经存在
18019    | 无效的回调URL

### 返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "order_id":"166",
          "order_hash_id":"d5d723f2fcf3b2f1d7f7f4ddfbd22f9ee6e76c3f080a6ce45f1a7d7219584c3b",
          "order_type":"200",
          "handle_status":"100",
          "user_id":"17",
          "external_order_id":"5",
          "external_callback_url":"http://10.0.1.254/app_demo/callback.php",
          "external_redirect_url":"",
          "external_info":"",
          "seller_hint":"",
          "product_id":"-1",
          "display_name":"这是我的第一个测试订单",
          "display_desc":"这是我的第一个测试订单，希望能给我赚很多比特币。内容请不要带有URL字符，会被过滤掉。",
          "price_btc":"100000000",
          "price_cny":"0",
          "pay_user_id":"-1",
          "pay_btc":"100000000",
          "ratio_btc2cny":"372097",
          "onchain_receive_btc_address":"13GvG56F2XhdD78ejRVVoP1pq7BGN88tRx",
          "onchain_leave_message":"",
          "onchain_notify_phone":"",
          "onchain_notify_email":"",
          "onchain_btc_deposit_id":"-1",
          "offchain_btc_transfer_request_id":"-1",
          "offchain_leave_message":"",
          "order_receipt_id":"371915798225",
          "creation_time":"2014-03-08 10:36:17",
          "last_modify_time":"2014-03-08 10:36:17"
       },
       "exec_time":"0.8294"
    }




## 订单详情  /order/detail/
订单详情 
### 请求路径： <code>/order/detail/</code>
### 参数表

**名称**  |  **类型** | **必须** | **说明**
------------- | ---------- | ---------- | -------------
order_hash_id | string    | 是   |     订单Hash ID

### 错误码

**Code** | **Mesasge**
-------- | ------------------------------------
18008    | 订单不存在
18009    | 订单对应的内部商品不存在


### 成功返回(该示例关联了内部商品)

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "order_id":"1",
          "order_hash_id":"ce60f638204ba900314a3af8c2474b299fcea7beb61f5cb1f930b2e785310a26",
          "handle_status":"100",
          "user_id":"13",
          "external_order_id":"7d080f1496fc011373a6b19c5a9f73affe2a4ec1ba14aefc563f95198ade3fe1",
          "external_info":"",
          "product_id":"6",
          "display_name":"参数值需要urlencode()",
          "display_desc":"",
          "price_btc":"0",
          "price_cny":"100000",
          "pay_user_id":"-1",
          "pay_btc":"-1",
          "ratio_btc2cny":"-1",
          "onchain_receive_btc_address":"14ntNhi3gzHKXSp45DyhgkBtsr5eG7WdHg",
          "onchain_leave_message":"",
          "onchain_notify_phone":"",
          "onchain_notify_email":"",
          "onchain_btc_deposit_id":"-1",
          "offchain_btc_internal_transfer_id":"-1",
          "creation_time":"2014-02-28 08:17:45",
          "last_modify_time":"2014-02-28 08:17:45",
          "product":{
             "product_id":"6",
             "product_hash_id":"348101df8114dae2fcf8e71093ef4ef31e7e780dcaa709b9e060fb5c48b4226d",
             "user_id":"13",
             "product_name":"参数值需要urlencode()",
             "product_desc":"",
             "price_btc":"0",
             "price_cny":"100000",
             "creation_time":"2014-02-28 06:51:44",
             "last_modify_time":"2014-02-28 06:51:44"
          }
       },
       "exec_time":"0.2006"
    }



## 订单更新  /order/update/
更新订单的留言等信息

### 请求路径： <code>/order/update/</code>
### 参数表

**名称**      |      **类型** | **必须** | **说明**
--------------------- | ---------- | ---------- | ----------------------
order_hash_id         | string     | 是         | 订单Hash ID
onchain_leave_message | string     | 是         | onchain支付留言
onchain_notify_phone  | string     | 是         | onchain通知手机号
onchain_notify_email  | string     | 是         | onchain通知邮箱

### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.2471"
    }



## 订单详情（通过外部ID） /order/query/
通过external_order_id查看订单详情，用于检查订单支付状态等

### 请求路径： <code>/order/query/</code>

**名称**  |  **类型** | **必须** | **说明**
------------- | ---------- | ---------- | -------------
external_order_id | string    | 是   |     订单外部 ID

### 错误码

**Code** | **Mesasge**
-------- | ------------------------------------
18008    | 订单不存在
18009    | 订单对应的内部商品不存在


### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "order_id":"1",
          "order_hash_id":"ce60f638204ba900314a3af8c2474b299fcea7beb61f5cb1f930b2e785310a26",
          "handle_status":"100",
          "user_id":"13",
          "external_order_id":"7d080f1496fc011373a6b19c5a9f73affe2a4ec1ba14aefc563f95198ade3fe1",
          "external_info":"",
          "product_id":"6",
          "display_name":"参数值需要urlencode()",
          "display_desc":"",
          "price_btc":"0",
          "price_cny":"100000",
          "pay_user_id":"-1",
          "pay_btc":"-1",
          "ratio_btc2cny":"-1",
          "onchain_receive_btc_address":"14ntNhi3gzHKXSp45DyhgkBtsr5eG7WdHg",
          "onchain_leave_message":"",
          "onchain_notify_phone":"",
          "onchain_notify_email":"",
          "onchain_btc_deposit_id":"-1",
          "offchain_btc_internal_transfer_id":"-1",
          "creation_time":"2014-02-28 08:17:45",
          "last_modify_time":"2014-02-28 08:17:45"
       },
       "exec_time":"0.2006"
    }

商品
======

## 创建商品 /product/create/
创建一个用与付款的商品
### 请求路径： <code>/product/create/</code>
### 参数表

**名称**  | **类型** | **必须**            |                     **说明**
------------ | ---------- | ------------------------------------------------ | ---------------------------------------------------------
product_name | string     | 是                                    |           商品名称。100字符以内
product_desc | string     | 否                                    |           商品描述。200字符以内
price_btc    | number     | 是(price_btc/price_cny必须有一个大于零) | 比特币定价，单位btc。范围：[0.0001, 20000000]
price_cny    | number     | 是(price_btc/price_cny必须有一个大于零) | 人民币定价，单位元。范围：[1, 1000000000]
seller_hint  | string     | 否                                    |           卖家提示（100字以内）
is_leave_message_required | int | 否                           | int 0或1 ,是否强制客户输入留言
quantity_discount[][quantity] | int | 否                                | 折扣对应数量数组,如quantity_discount[0][quantity]=10
quantity_discount[][discount] | int | 否                                | 折扣对应折扣数组,90代表9折,如quantity_discount[0][discount]=90
callback_url | string     | 否                                | 回调URL
redirect_url | string     | 否                                | 提示跳转URL


### 错误码

**Code** | **Mesasge**
-------- | ---------------------------------------------------------------
18000    | 商品名为空
18001    | 无效的比特币定价
18002    | 无效的人民币定价
18003    | 多个定价均为零
18004    | 保存至数据库失败
18015    | 比特币、人民币价格同时出现，仅允许一个出现


### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "0":{
             "product_id":"6",
             "product_hash_id":"348101df8114dae2fcf8e71093ef4ef31e7e780dcaa709b9e060fb5c48b4226d",
             "user_id":"13",
             "product_name":"参数值需要urlencode()",
             "product_desc":"",
             "price_btc":"0",
             "price_cny":"100000",
             "creation_time":"2014-02-28 06:51:44",
             "last_modify_time":"2014-02-28 06:51:44"
          }
       },
       "exec_time":"0.2112"
    }



## 商品存档  /product/archive/
将商品存档，一旦存档，则无法支付相关的订单
### 请求路径： <code>/product/archive/</code>
### 参数表

**名称** | **类型** | **必须** | **默认** | **说明**
---------- | ---------- | ---------- | ---------- | ----------
product_id | int        | 是         | 无          | 商品ID
### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.2471"
    }

### 失败返回

    {
       "error_no":0,
       "error_msg":"",
       "result":0,
       "exec_time":"0.2471"
    }



## 商品详情  /product/detail/
查看商品
### 请求路径： <code>/product/detail/</code>
### 参数表

**名称** | **类型** | **必须** | **默认** | **说明**
---------- | ---------- | ---------- | ---------- | ----------
product_id | int        | 是         | 无         | 商品ID

### 错误码

**Code** | **Mesasge**
-------- | ---------------------
18005    | 无法查询该商品


### 成功返回

    {
    "error_no": 0, 
    "error_msg": "", 
    "result": {
        "product_id": "128", 
        "product_hash_id": "8d7c95cb90aa745a171be1acee81967abd4177420f0ca48a6b14e8c0928fe406", 
        "is_archive": "0", 
        "user_id": "34", 
        "external_order_id": "", 
        "callback_url": "http://bifubao.com", 
        "redirect_url": "http://www.bifubao.com", 
        "product_name": "jiaoqian", 
        "product_desc": "shouqian", 
        "price_btc": "120000", 
        "price_cny": "0", 
        "seller_hint": "", 
        "quantity_discount": [
            {
                "quantity": "10000", 
                "discount": "90"
            }
        ], 
        "is_quantity_changeable": "1", 
        "is_leave_message_required": "1", 
        "creation_time": "2014-06-17 10:04:51", 
        "last_modify_time": "2014-06-17 10:04:51"
    }, 
    "exec_time": "0.1697"
}




## 商品列表 /product/list
查看创建的的商品
### 请求路径： <code>/product/list/</code>
### 参数表

**名称** | **类型** | **必须** | **默认** | **说明**
---------- | ---------- | ---------- | ---------- | --------------------------------
page_no    | int        | 是         |  1          | 分页码
page_size  | int        | 否         | 500         |  分页大小，范围：[2, 500]

### 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "total_count":1,
          "page_no":1,
          "page_size":500,
          "items":{
             "0":{
                "product_id":"1",
                "product_hash_id":"990f0d09fb282f2853505609fbfd01eb9f0f8d5b9f9cf6ee03f3256e70f4ae13",
                "user_id":"13",
                "product_name":"，自动填入右侧，供后续",
                "product_desc":"product_descproduct_desc",
                "price_btc":"11111112345678",
                "price_cny":"0",
                "creation_time":"2014-02-28 06:06:10",
                "last_modify_time":"2014-02-28 06:06:10"
             }
          }
       },
       "exec_time":"0.2007"
    }



余额
=====

## 余额变更详情 /balance/detail/
含义：余额变更详情
### 请求路径： <code>/balance/detail/</code>
### 参数表

**名称** | **类型** | **说明**
---------- | ---------- | ----------
balance_id | INT        | balance_id


### 返回: deposit

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "balance_id":"250",
          "prev_balance_id":"228",
          "user_id":"31",
          "event_date":"2014-03-03",
          "change_event_type":"100",
          "change_event_id":"13",
          "diff_balance":"10000",
          "prev_balance":"99899890000",
          "cur_balance":"99899900000",
          "related_user_id":"0",
          "related_btc_address":"",
          "creation_time":"2014-03-03 13:20:36",
          "related_event":{
             "btc_deposit_id":"13",
             "tx_hash":"b1c77de194ff5d142700f31ec332f68a6fe26bd7fc03a301b49e66af5ebeb409",
             "receive_btc_address":"1NmZM7Ema6ydzJb9sTWsUqcCRfuRtKbcsE",
             "order_id":"",
             "order_hash_id":"",
             "order_display_name":"",
             "order_display_desc":"",
             "order_price_btc":"",
             "order_ratio_btc2cny":"",
             "order_offchain_leave_message":"",
             "order_seller_hint":"",
             "order_product_id":"",
             "order_receipt_id":"",
             "order_onchain_btc_deposit_id":"",
             "order_offchain_btc_transfer_request_id":""
          },
          "tx_hash":"b1c77de194ff5d142700f31ec332f68a6fe26bd7fc03a301b49e66af5ebeb409",
          "receive_btc_address":{
             "btc_address":"1NmZM7Ema6ydzJb9sTWsUqcCRfuRtKbcsE",
             "label":"测试主"
          }
       },
       "exec_time":"0.0555"
    }


### 返回: withdrawal

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "balance_id":"228",
          "prev_balance_id":"212",
          "user_id":"31",
          "event_date":"2014-03-03",
          "change_event_type":"400",
          "change_event_id":"4",
          "diff_balance":"-110000",
          "prev_balance":"99900000000",
          "cur_balance":"99899890000",
          "related_user_id":"0",
          "related_btc_address":"",
          "creation_time":"2014-03-03 10:10:19",
          "related_event":{
             "btc_withdrawal_id":"4",
             "tx_hash":"41638ad944b0d45be66f32bbf9e5767edb2b1067bde25a50836907303b44e05e",
             "receive_btc_address":"1BckJD1tSaEQ2PpoC1pxAwF5GWv3FpeTsd",
             "leave_message":"",
             "handle_status":"1000"
          },
          "tx_hash":"41638ad944b0d45be66f32bbf9e5767edb2b1067bde25a50836907303b44e05e",
          "receive_btc_address":{
             "btc_address":"1BckJD1tSaEQ2PpoC1pxAwF5GWv3FpeTsd"
          },
          "handle_status":"1000"
       },
       "exec_time":"0.0734"
    }


### 返回: 内部转账
    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "balance_id":"212",
          "prev_balance_id":"185",
          "user_id":"31",
          "event_date":"2014-03-03",
          "change_event_type":"200",
          "change_event_id":"207",
          "diff_balance":"-100000000",
          "prev_balance":"100000000000",
          "cur_balance":"99900000000",
          "related_user_id":"5",
          "related_btc_address":"",
          "creation_time":"2014-03-03 07:35:54",
          "related_event":{
             "btc_internal_transfer_id":"207",
             "user_id":"31",
             "target_user_id":"5",
             "leave_message":"发送给同事",
             "order_id":"",
             "order_hash_id":"",
             "order_display_name":"",
             "order_display_desc":"",
             "order_price_btc":"",
             "order_ratio_btc2cny":"",
             "order_offchain_leave_message":"",
             "order_seller_hint":"",
             "order_product_id":"",
             "order_receipt_id":"",
             "order_onchain_btc_deposit_id":"",
             "order_offchain_btc_transfer_request_id":""
          },
          "related_user":{
             "user_id":"5",
             "user_email":"test@example.com",
             "mobile_phone_num":"",
             "contact":{
                "user_contact_id":"137",
                "contact_user_id":"5",
                "label":"",
                "transfer_counter":"1"
             }
          }
       },
       "exec_time":"0.0919"
    }


    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "balance_id":"185",
          "prev_balance_id":"-31",
          "user_id":"31",
          "event_date":"2014-03-01",
          "change_event_type":"300",
          "change_event_id":"184",
          "diff_balance":"100000000000",
          "prev_balance":"0",
          "cur_balance":"100000000000",
          "related_user_id":"0",
          "related_btc_address":"",
          "creation_time":"2014-03-01 06:47:45",
          "related_event":{
             "btc_internal_transfer_id":"184",
             "user_id":"31",
             "target_user_id":"5",
             "leave_message":"hello",
             "order_id":"",
             "order_hash_id":"",
             "order_display_name":"",
             "order_display_desc":"",
             "order_price_btc":"",
             "order_ratio_btc2cny":"",
             "order_offchain_leave_message":"",
             "order_seller_hint":"",
             "order_product_id":"",
             "order_receipt_id":"",
             "order_onchain_btc_deposit_id":"",
             "order_offchain_btc_transfer_request_id":""
          },
          "related_user":{
             "user_id":"5",
             "user_email":"test@example.com",
             "mobile_phone_num":"",
             "contact":{
                "user_contact_id":"137",
                "contact_user_id":"5",
                "label":"",
                "transfer_counter":"1"
             }
          }
       },
       "exec_time":"0.0433"
    }



通知
======

## 更新通知 /notify/update/
更新用户通知配置
### 请求路径： <code>/notify/update/</code>
### 参数表

**名称** | **类型**   |    **说明**
---------- | ---------------- | -------------------
event_101  | int，（0or1）     | 收款,手机短信
event_102  | int，（0or1）     | 收款,邮件
event_201  | int，（0or1）     | 发款,手机短信
event_202  | int，（0or1）     | 发款,邮件
event_301  | int，（0or1）     | 订单,手机短信
event_302  | int，（0or1）     | 订单,邮件

### 返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.1323"
    }



## 获取通知设置详情 /notify/detail/
获取通知设置详情
### 请求路径： <code>/notify/detail/</code>
已存在记录时，返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "notify_settting_id":"1",
          "user_id":"31",
          "event_101":"1",
          "event_102":"1",
          "event_201":"1",
          "event_202":"1",
          "event_301":"1",
          "event_302":"1",
          "creation_time":"2014-03-18 06:26:29",
          "last_modify_time":"2014-03-18 06:28:33"
       },
       "exec_time":"0.0592"
    }

若不存在记录，返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "user_id":31,
          "event_101":1,
          "event_102":1,
          "event_201":1,
          "event_202":1,
          "event_301":0,
          "event_302":0
       },
       "exec_time":"0.0971"
    }



移动端信息
===========

## 获取版本号和下载地址 /mobile/version/
获取版本号和下载地址
### 请求路径： <code>/mobile/version/</code>
### 返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "android":{
             "version_int":1,
             "version_str":"v0.0.1",
             "version_desc":"第一版",
             "download_url":"http://static.bifubao.com/dl/andriod-v0.0.1.apk"
          }
       },
       "exec_time":"0.0048"
    }





移动端支付PIN码
===============

## 创建PIN码 /paypincode/create/
创建PIN码
### 请求路径： <code>/paypincode/create/</code>
### 参数表

**名称**  | **类型**      |     **说明**
------------ | -------------------- | ---------------------
pay_pin_code | string, 六位数字      | 移动端支付PIN码

### 错误码

**Code** | **Mesasge**
-------- | -------------------------------
20000    | pay_pin_code必须为6位数字
20001    | 用户已经设置pay_pin_code
20002    | 存储至数据库失败
20004    | 弱密码或者生日



## 重置PIN码 /paypincode/reset/
重置PIN码
### 请求路径： <code>/paypincode/reset/</code>
### 参数表

**名称**     |  **类型**             | **说明**
------------ | -------------------- | -------------------------------------------------
pay_pin_code | string, 六位数字      | 移动端支付PIN码
ga_code      | number               | 是（ga_code/pin_code必须有一个不为空） google二次验证码
pin_code     | number               | 是（ga_code/pin_code必须有一个不为空） 短信（文字/语音）验证码


### 错误码

**Code** | **Mesasge**
-------- | ---------------------------------
20003    | 该用户尚未设置pay_pin_code
20004    | 弱密码或者生日




理财产品
========

购买理财产品 /financial/deposit/
-------------------------------
购买理财产品，即存入

### 请求路径： ```/financial/deposit/```
### 参数表

| *名称*               | *类型* | *必须* | *说明*                                                  |
|----------------------|-------|-------|---------------------------------------------------------| 
| financial_product_id | int   | 是    | 所购买理财产品的ID                                        |
| btc_amount           | number | 是   | 所购买的的数量 转账金额，单位btc。range: [0.0001, 20000000] |
| ga_code        | number     | 是（ga_code/pin_code必须有一个不为空，web端）         |  google二次验证码 |
| pin_code       | number     | 是（ga_code/pin_code必须有一个不为空，web端）         |  短信（文字/语音）验证码 |
| pay_pin_code   | number     | 是（移动端不填写ga_code/pin_code，须填写pay_pin_code） | 移动端支付PIN码 |


### 错误码

| *Code* | *Mesasge* |
|--------|-----------|
| 22000 | 通过financial_product_id找不到所要购买的理财产品 |
| 22001| 用户余额不足以支付 |
| 22002 | 找不到对应的正确的交易历史 |
| 16002 | 超出范围 [0.0001, 20000000] |
| 22003 | 存入总数量大于理财产品预设最大存入值 |
| 22012 | 不能给自己发起的理财产品存款 |

### 返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "btc_transfer_request_id":"687",
          "financial_history_id":"279"
       },
       "exec_time":"0.4422"
    }



兑现理财产品 /financial/withdraw/
--------------------------------
兑现理财产品，即取出

### 请求路径：```/financial/withdraw/```

### 参数表
| *名称* | *类型* | *必须* | *说明* |
|-------|--------|-------|--------|
| financial_product_id | integer | 是 | 所购买理财产品的ID |
| btc_amount | number | 是 | 所购买的的数量 转账金额，单位btc。range: [0.0001, 20000000] |
| ga_code        | number     | 是（ga_code/pin_code必须有一个不为空，web端）         |  google二次验证码 |
| pin_code       | number     | 是（ga_code/pin_code必须有一个不为空，web端）         |  短信（文字/语音）验证码 |
| pay_pin_code   | number     | 是（移动端不填写ga_code/pin_code，须填写pay_pin_code） | 移动端支付PIN码 |


### 错误码

| *Code* | *Mesasge* |
|--------|-----------|
| 22000 | 通过financial_product_id找不到所要购买的理财产品 |
| 22010| 用户理财产品余额不足 |

### 返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":"29",
       "exec_time":"0.2120"
    }




理财产品历史记录列表 /financial/historylist/
---------------------------------------------
查看理财产品历史记录

### 请求路径： ```/financial/historylist/```

### 参数表

| *名称* | *类型* | *必须* | *默认* | *说明* |
|-------|--------|--------|-------|-------|
| page_no | int  | 是 | 1 | 分页码 |
| page_size | int  | 否 | 500 | 分页大小，范围：[2, 500] |

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "total_count":58,
          "page_no":1,
          "page_size":3,
          "items":{
             "0":{
                "id":"281",
                "financial_product_id":"1",
                "user_id":"17",
                "amount":"-99980000",
                "type":"300",
                "balance":"50270716719",
                "interest_rate":{
                   
                },
                "withdraw_fee_rate":{
                   
                },
                "handle_status":"1000",
                "created_at":"2014-09-04 08:56:16",
                "updated_at":"2014-09-04 08:56:16",
                "type_display":"withdraw",
                "handle_status_display":"success"
             },
             "1":{
                "id":"280",
                "financial_product_id":"1",
                "user_id":"17",
                "amount":"-20000",
                "type":"350",
                "balance":"50370696719",
                "interest_rate":{
                   
                },
                "withdraw_fee_rate":"0.0002",
                "handle_status":"1000",
                "created_at":"2014-09-04 08:56:15",
                "updated_at":"2014-09-04 08:56:15",
                "type_display":"withdraw fee",
                "handle_status_display":"success"
             },
             "2":{
                "id":"279",
                "financial_product_id":"1",
                "user_id":"17",
                "amount":"100000000",
                "type":"100",
                "balance":"50370716719",
                "interest_rate":{
                   
                },
                "withdraw_fee_rate":{
                   
                },
                "handle_status":"1000",
                "created_at":"2014-09-04 08:53:07",
                "updated_at":"2014-09-04 08:53:08",
                "type_display":"deposit",
                "handle_status_display":"success"
             }
          }
       },
       "exec_time":"0.0482"
    }



用户理财产品信息 /financial/userproduct/
--------------------------------------------
查看用户理财产品信息

### 请求路径： ```/financial/userproduct/```

### 参数表

| *名称* | *类型* | *必须* | *默认* | *说明* |
|-------|--------|-------|--------|--------|
| financial_product_id | int  | 是 | 1 | 理财产品的id |

### 返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "id":"2",
          "financial_product_id":"1",
          "user_id":"17",
          "current_balance":"50270716719",
          "withdraw_pending_balance":"0",
          "deposit_pending_balance":"0",
          "is_locked":"0",
          "last_lock_time":"2014-09-04 08:56:15",
          "created_at":"2014-07-21 00:00:00",
          "updated_at":"2014-09-04 08:56:15"
       },
       "exec_time":"0.0832"
    }




用户有效理财产品列表 /financial/userproductlist/
--------------------------------------------
用户有效理财产品列表

### 请求路径： ```/financial/userproductlist/```

### 参数表

| *名称* | *类型* | *必须* | *默认* | *说明* |
|-------|--------|-------|--------|--------|
| page_no | int  | 是 | 1 | 分页码 |
| page_size | int  | 否 | 500 | 分页大小，范围：[2, 500] |

### 返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "total_count":1,
          "page_no":1,
          "page_size":500,
          "items":{
             "0":{
                "id":"2",
                "financial_product_id":"1",
                "user_id":"17",
                "current_balance":"50270716719",
                "withdraw_pending_balance":"0",
                "deposit_pending_balance":"0",
                "is_locked":"0",
                "last_lock_time":"2014-09-04 08:56:15",
                "created_at":"2014-07-21 00:00:00",
                "updated_at":"2014-09-04 08:56:15"
             }
          }
       },
       "exec_time":"0.0988"
    }




等待提款列表 /financial/withdrawlist/
--------------------------------------------
查看在等待出款的提款

### 请求路径： ```/financial/withdrawlist/```

### 参数表

| *名称* | *类型* | *必须* | *默认* | *说明* |
|-------|--------|-------|--------|--------|
| page_no | int  | 是 | 1 | 分页码 |
| page_size | int  | 否 | 500 | 分页大小，范围：[2, 500] |

### 返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "total_count":1,
          "page_no":1,
          "page_size":500,
          "items":{
             "0":{
                "id":"30",
                "financial_product_id":"1",
                "user_id":"17",
                "amount":"100000000",
                "handle_status":"500",
                "withdraw_effect_at":"1409821317",
                "withdraw_effect_at_datetime":"2014-09-04 09:01:57",
                "created_at":"2014-09-04 09:00:56",
                "updated_at":"2014-09-04 09:00:57",
                "handle_status_display":"processing"
             }
          }
       },
       "exec_time":"0.0647"
    }





理财产品列表 /financial/productlist/
--------------------------------------------
查看目前可用的理财产品

### 请求路径： ```/financial/productlist/```

### 参数表

| *名称* | *类型* | *必须* | *默认* | *说明* |
|-------|--------|-------|--------|--------|
| page_no | int  | 是 | 1 | 分页码 |
| page_size | int  | 否 | 500 | 分页大小，范围：[2, 500] |

### 返回：
    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "total_count":1,
          "page_no":1,
          "page_size":500,
          "items":{
             "0":{
                "id":"1",
                "name":"BitcoinSand",
                "icon_url":"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACgAAAAoCAYAAACM/rhtAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyJpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMC1jMDYwIDYxLjEzNDc3NywgMjAxMC8wMi8xMi0xNzozMjowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNSBNYWNpbnRvc2giIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6RkM4MThENjVFRDMzMTFFMjg0OTRFRjM0MUU0QzZDRkMiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6RkM4MThENjZFRDMzMTFFMjg0OTRFRjM0MUU0QzZDRkMiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpGQzgxOEQ2M0VEMzMxMUUyODQ5NEVGMzQxRTRDNkNGQyIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpGQzgxOEQ2NEVEMzMxMUUyODQ5NEVGMzQxRTRDNkNGQyIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PqPC3mAAAAFSSURBVHjazNi9CsIwEAfwxFMyuTjpID6Dj+BjuOmLOPgi9k2c3F0FnVUEEcVBFDTJUPCjzd3FtncQujTpr5A/7aWuCNWebGf2MlL8SnbTzpgyASTj0MCqcChglbggsGpcLlACLhMoBfcTKAn3BZSGewNKxKVAqTgPlIxzpS3wyZ18O2zUZb3IvsE07ROAjWv2+qpWGK4G0TjT6vKAQZzfPCYa59+zEJzWFtiIxpGBKJxf1eF0NI4ERONc1c1fcGggCccIRxYOBSThGOHIwwWBZBwxHCFcLpCMI4YDg8sEsnCEcGBxP4FsHDIcFNwXkI1DhoOKewNG4RDh4OBS4P285+MQ4eDi/NKP61GdV/O4n7accMTgFOuDWe5ZzhIk4+wYgGScbSVOIBCX2DF0OL+9peE+GzCQjPsbsMjWFSTjooFlNP0gGccGlnlcApJxrl4CDADuItjAFkT1xAAAAABJRU5ErkJggg==",
                "description":"全球第一家比特币银行业务，现正式与币付宝合作，日利息约 0.01%，年化收益率约 3.8%",
                "description_en":"Bitcoinsand is the world's first bitcoin bank. As a partner of Bitfoo, Bitcoinsand offers a daily interest of 0.01%, i.e. 3.8% per year.",
                "owner_user_id":"61",
                "interest_rate":"0.0001",
                "withdraw_pending_days":"1",
                "interest_init_amount":"10000000",
                "max_deposit_amount":"100000000000",
                "quota":"3000000000000",
                "withdraw_fee_rate":"0.0002",
                "interest_interval_days":"1",
                "interest_status":"100",
                "interest_release_time":"2014-09-04 04:00:00",
                "interest_next_release_time":"2014-09-05 04:00:00",
                "created_at":"2014-07-21 16:38:02",
                "updated_at":"2014-09-04 04:00:00"
             }
          }
       },
       "exec_time":"0.0867"
    }




理财产品是否存在变动 /financial/productchange/
--------------------------------------------
是否存在计划中的理财产品变动，如利率调整等

### 请求路径： ```/financial/productchange/```

### 参数表

| *名称* | *类型* | *必须* | *默认* | *说明* |
|-------|--------|-------|--------|--------|
| financial_product_id | int  | 是 | 1 | 理财产品的id |

### 返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":false,
       "exec_time":"0.0622"
    }




理财产品详情 /financial/productdetail/
--------------------------------------------
查看某一个具体的理财产品的详情

### 请求路径： ```/financial/productdetail/```

### 参数表

| *名称* | *类型* | *必须* | *默认* | *说明* |
|-------|--------|-------|--------|--------|
| financial_product_id | int  | 是 | 1 | 理财产品的id |


### 返回：
    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "id":"1",
          "name":"BitcoinSand",
          "icon_url":"data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACgAAAAoCAYAAACM/rhtAAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyJpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMC1jMDYwIDYxLjEzNDc3NywgMjAxMC8wMi8xMi0xNzozMjowMCAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNSBNYWNpbnRvc2giIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6RkM4MThENjVFRDMzMTFFMjg0OTRFRjM0MUU0QzZDRkMiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6RkM4MThENjZFRDMzMTFFMjg0OTRFRjM0MUU0QzZDRkMiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpGQzgxOEQ2M0VEMzMxMUUyODQ5NEVGMzQxRTRDNkNGQyIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpGQzgxOEQ2NEVEMzMxMUUyODQ5NEVGMzQxRTRDNkNGQyIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PqPC3mAAAAFSSURBVHjazNi9CsIwEAfwxFMyuTjpID6Dj+BjuOmLOPgi9k2c3F0FnVUEEcVBFDTJUPCjzd3FtncQujTpr5A/7aWuCNWebGf2MlL8SnbTzpgyASTj0MCqcChglbggsGpcLlACLhMoBfcTKAn3BZSGewNKxKVAqTgPlIxzpS3wyZ18O2zUZb3IvsE07ROAjWv2+qpWGK4G0TjT6vKAQZzfPCYa59+zEJzWFtiIxpGBKJxf1eF0NI4ERONc1c1fcGggCccIRxYOBSThGOHIwwWBZBwxHCFcLpCMI4YDg8sEsnCEcGBxP4FsHDIcFNwXkI1DhoOKewNG4RDh4OBS4P285+MQ4eDi/NKP61GdV/O4n7accMTgFOuDWe5ZzhIk4+wYgGScbSVOIBCX2DF0OL+9peE+GzCQjPsbsMjWFSTjooFlNP0gGccGlnlcApJxrl4CDADuItjAFkT1xAAAAABJRU5ErkJggg==",
          "description":"全球第一家比特币银行业务，现正式与币付宝合作，日利息约 0.01%，年化收益率约 3.8%",
          "description_en":"Bitcoinsand is the world's first bitcoin bank. As a partner of Bitfoo, Bitcoinsand offers a daily interest of 0.01%, i.e. 3.8% per year.",
          "owner_user_id":"61",
          "interest_rate":"0.0001",
          "withdraw_pending_days":"1",
          "interest_init_amount":"10000000",
          "max_deposit_amount":"100000000000",
          "quota":"3000000000000",
          "withdraw_fee_rate":"0.0002",
          "interest_interval_days":"1",
          "interest_status":"100",
          "interest_release_time":"2014-09-04 04:00:00",
          "interest_next_release_time":"2014-09-05 04:00:00",
          "created_at":"2014-07-21 16:38:02",
          "updated_at":"2014-09-04 04:00:00",
          "owner_user_verify_name":"bitcoinsand"
       },
       "exec_time":"0.1009"
    }



理财产品利息发放历史 /financial/interesthistory/
--------------------------------------------
查看近6次的理财产品利息发放情况，用户图表绘制

### 请求路径： ```/financial/interesthistory/```

### 参数表

| *名称* | *类型* | *必须* | *默认* | *说明* |
|-------|--------|-------|--------|--------|
| financial_product_id | int  | 是 | 1 | 理财产品的id |

### 返回：
    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "date":{
             "0":"2014-08-29 04:00:03",
             "1":"2014-08-30 04:00:01",
             "2":"2014-08-31 04:00:01",
             "3":"2014-09-01 04:00:02",
             "4":"2014-09-02 04:00:02",
             "5":"2014-09-03 04:00:01",
             "6":"2014-09-04 04:00:02"
          },
          "amount":{
             "0":"0.05023554",
             "1":"0.05024056",
             "2":"0.05024558",
             "3":"0.05025061",
             "4":"0.05025563",
             "5":"0.05026066",
             "6":"0.05026569"
          }
       },
       "exec_time":"0.0921"
    }


币券
=========

创建币券 /redemption/create/
---------------------------------------
创建币券

### 请求路径： ```/redemption/create/```

### 参数表
| *名称* | *类型* | *必须* | *说明* |
|-------|--------|-------|--------|
| total_amount_btc | float | 是 | 币券额度之和，单位：比特币, btc |
| count | int | 是 | 币券数量 |
| expire_days | int | 是, range: [1, 30] | 过期天数，默认3天 |
| ga_code | number |  是（ga_code/pin_code必须有一个不为空） | google二次验证码 | 
| pin_code | number |  是（ga_code/pin_code必须有一个不为空） | 短信（文字/语音）验证码 | 
| expire_timezone | int | 否，默认：0 | 过期时间的时区，建议对用户隐藏，通过浏览器JS可以直接拿到时区 |
| title | string，30字符以内 | 是 | 币券标题 |
| is_anti_greed | int | 否 | int 0 或 1, 1 表示所生成的币券批次内每张仅允许一个用户兑换一次 |
| send_to | string | 否 | 指定的兑换人，内容为邮箱或手机 |
| uniqkey | string | 是 | API v2后为必须，唯一键用于csrf、重放攻击等 |

   * pin_code类型为：310，创建币券


### 错误码
| *Code* | *Mesasge* |
|--------|-----------|
| 21000 | total_amount过低，无法分配（低于一聪） |
| 21001 | count无效，允许范围：[1, 2000] |
| 21002 | 无效的标题，范围：[1, 30] |
| 21003 | expire_days过期天数无效，不在范围内 |
| 21004 | expire_timezone过期时区无效 |
| 21005 | 用户余额不足 |
| 21007 | 超过最大可创建数量 |

### 内部流程
1. 创建一个redemption_batch
1. 由batch生成具体的codes



提取币券 /redemption/redeem/
----------------------------------------
提取币券

### 请求路径： ```/redemption/redeem/```

### 参数表
| *名称* | *类型* | *必须* | *说明* |
|-------|--------|-------|--------|
| code | string | 是 | 币券码，形如：xxxx-xxxx-xxxx-xxxx(1056-4188-2979-8351) |

### 错误码
| *Code* | *Mesasge* |
|--------|-----------|
| 21010 | 无效的code: 不存在、或已过期 |
| 21011 | 已被提取 |


### 返回信息, 成功返回
    {
       "error_no":0,
       "error_msg":"",
       "result":1,
       "exec_time":"0.3811"
    }

查看币券详情 /redemption/codedetail/
----------------------------------------
查看币券详情

### 请求路径： ```/redemption/codedetail/```

### 参数表
| *名称* | *类型* | *必须* | *说明* |
|-------|--------|-------|--------|
| code | string | 是 | 币券码，形如：xxxx-xxxx-xxxx-xxxx(1056-4188-2979-8351) |

### 错误码
| *Code* | *Mesasge* |
|--------|-----------|
| 21010 | 无效的code: 不存在、或已过期 |

### 返回信息
成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "title":"币付宝测试币券",
          "has_expired":"0",
          "has_redeemed":"0",
          "has_refunded":"0",
          "amount":"10000000",
          "amount_btc":"0.10000000",
          "code":"2677-8544-4925-7118",
          "expire_timezone":"8",
          "expire_time_int":"1399737601",
          "source_user_email":"p*****u@gmail.com",
          "source_user_mobile_phone_num":"",
          "send_to":""
       },
       "exec_time":"0.0059"
    }

失败返回

    {
       "error_no":21010,
       "error_msg":"invalid redemption code or expired",
       "result":{
          
       },
       "exec_time":"0.0112"
    }



币券批次详情 /redemption/batchdetail/
----------------------------------------
查看币券批次

### 请求路径： ```/redemption/batchdetail/```

### 参数表
| *名称* | *类型* | *必须* | *说明* |
|-------|--------|-------|--------|
| redemption_batch_id | int | 是 | redemption_batch_id |

### 错误码
| *Code* | *Mesasge* |
|--------|-----------|

### 返回信息, 成功返回

    array(4) {
      ["error_no"]=>
      int(0)
      ["error_msg"]=>
      string(0) ""
      ["result"]=>
      array(12) {
        ["redemption_batch_id"]=>
        string(1) "6"
        ["handle_status"]=>
        string(4) "1000"
        ["user_id"]=>
        string(1) "5"
        ["total_amount"]=>
        string(9) "100000000"
        ["count"]=>
        string(2) "10"
        ["left_count"]=>
        string(2) "10"
        ["expire_days"]=>
        string(2) "30"
        ["expire_timezone"]=>
        string(1) "8"
        ["expire_time_int"]=>
        string(10) "1399737601"
        ["title"]=>
        string(18) "币付宝测试币券"
        ["creation_time"]=>
        string(19) "2014-04-11 08:05:52"
        ["last_modify_time"]=>
        string(19) "2014-04-11 08:05:56"
      }
      ["exec_time"]=>
      string(6) "0.0570"
    }


同批次币券列表 /redemption/codelist/
----------------------------------------
同批次币券列表

### 请求路径： ```/redemption/codelist/```

### 参数表
| *名称* | *类型* | *必须* | *说明* |
|-------|--------|-------|--------|
| redemption_batch_id | int | 是 | redemption_batch_id |

### 错误码
| *Code* | *Mesasge* |
|--------|-----------|

### 返回信息

成功返回
    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "0":{
             "has_redeemed":"1",
             "has_refunded":"0",
             "amount":"5000",
             "code":"6159-9856-5727-7419",
             "redeem_user":{
                "user_email":"4*****2@qq.com",
                "mobile_phone_num":""
             }
          },
          "1":{
             "has_redeemed":"1",
             "has_refunded":"0",
             "amount":"5000",
             "code":"8052-6108-3227-8495",
             "redeem_user":{
                "user_email":"4*****2@qq.com",
                "mobile_phone_num":""
             }
          },
          "2":{
             "has_redeemed":"0",
             "has_refunded":"0",
             "amount":"5000",
             "code":"7208-4657-3959-6870"
          },
          "3":{
             "has_redeemed":"0",
             "has_refunded":"0",
             "amount":"5000",
             "code":"1491-3733-9820-4791"
          },

          "199":{
             "has_redeemed":"0",
             "has_refunded":"0",
             "amount":"5000",
             "code":"4402-0547-9094-6235"
          }
       },
       "exec_time":"0.0597"
    }

无则返回空

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          
       },
       "exec_time":"0.0904"
    }


币券批次列表 /redemption/batchlist/
----------------------------------------
币券列表（批次）

### 请求路径： ```/redemption/batchlist/```

### 参数表
| *名称* | *类型* | *必须* | *说明* |
|-------|--------|-------|--------|
| page_no | int  | 是，默认：1 | 分页码 |
| page_size | int  | 否，默认：50 | 分页大小，范围：[2, 200] |

### 错误码
| *Code* | *Mesasge* |
|--------|-----------|

### 返回信息, 成功返回

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "total_count":2,
          "page_no":1,
          "page_size":50,
          "items":{
             "0":{
                "redemption_batch_id":"2",
                "handle_status":"1000",
                "user_id":"31",
                "total_amount":"1000000",
                "count":"200",
                "left_count":"198",
                "expire_days":"1",
                "expire_timezone":"8",
                "title":"测试2",
                "creation_time":"2014-04-10 06:18:05",
                "last_modify_time":"2014-04-10 09:37:49"
             },
             "1":{
                "redemption_batch_id":"1",
                "handle_status":"1000",
                "user_id":"31",
                "total_amount":"10000",
                "count":"100",
                "left_count":"0",
                "expire_days":"3",
                "expire_timezone":"8",
                "title":"测试1",
                "creation_time":"2014-04-08 08:39:28",
                "last_modify_time":"2014-04-10 06:10:47"
             }
          }
       },
       "exec_time":"0.0679"
    }

唯一键值
=======
防止重放攻击，CSRF等。默认过期时间为10分钟。

创建一个唯一键 /unikey/create/
-----------

### 参数表

**名称** | **类型** | **说明**
---------- | ---------- | ------------

### 错误码

**Code** | **Mesasge**
-------- | ----------------------------------------------------------------

### 成功返回：

    {
       "error_no":0,
       "error_msg":"",
       "result":{
          "uniqkey":"848d09a81834139b42c4b371af16552a"
       },
       "exec_time":"0.0060"
    }