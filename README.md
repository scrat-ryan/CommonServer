CommonServer

------

[TOC]

* [Header](#Header)
* [Config](#Config)
* [获取短信接口](#获取短信接口)
* [验证短信接口](#验证短信接口)
* [用户反馈接口](#用户反馈接口)

# Header

| param | type | require | description |
| :---: | :---: | :---: | --- |
| appid | int | true | app id |
| appkey | string | true | app key |

# Config

## nginx

```
location /api/core/sms {
    proxy_pass http://localhost:8041/api/sms;
}
location /api/core/feedback {
    proxy_pass http://localhost:8042/api/feedback;
}
```

# 获取短信接口

[GET] `/api/core/sms`

| param | type | require | description |
| :---: | :---: | :---: | --- |
| tel | string | true | 电话号码 |
| ts | long | true | 13位时间戳 |
| sign | string | true | md5("app_secret" + "ts" + "tel") |

Sample

```
curl -H 'appid: 1' -H 'appkey: 5651745861380' 'http://localhost:8041/api/sms?tel=17199916191&ts=1534132120765&sign=CD660D8F4E63C7E2E729B28817A6EF9D'
```

Response

```
{"code":200,"msg":"ok"}
```

# 验证短信接口

[POST] **application/json** `/api/core/sms`

| param | type | require | description |
| :---: | :---: | :---: | --- |
| tel | string | true | 电话号码 |
| code | int | true | 验证码 |

Sample

```
curl -X POST -H 'appid: 1234' -H 'appkey: 12345678' -H "Content-type: application/json" -d '{"tel":17199916191,"code":907826}' 'https://host/api/core/sms'
```

Response

```
{"code":200,"msg":"ok"}
```

# 用户反馈接口

[POST] **application/json** `/api/core/feedback`

| param | type | require | description |
| :---: | :---: | :---: | --- |
| uid | string | false | 用户ID |
| title | string | false | 标题 |
| content | string | true | 内容 |
| contract | string | false | 联系方式 |
| pt | string | false | 平台，可填入 iOS, Android, H5 等 |
| ch | string | false | 渠道，可填入 GooglePlay，AppStore，Official 等 |
| vc | string | false | 版本号 |
| vn | string | false | 版本名 |
| imgs | array | false | 图片列表，如 \["http://xxx.cn/xxx.png","http://xxx.cn/xxx.png" \] |

Sample

```
curl -X POST -H 'appid: 1' -H 'appkey: 5651745861380' -H "Content-type: application/json" -d '{"vc":"vc","vn":"vn","ch":"ch","pt":"pt","content":"content","uid":"uid","title":"title","contract":"contract","imgs":["http://baidu.com"]}' 'http://localhost:8042/api/feedback'
```

Response

```
{"code":200,"msg":"ok"}
```
