# 接入说明

## 相关信息

- APP接口项目：测试服务器目前部署在 `test10` 上、
- 接口负责人：`@jax、@ug_benjamin、@ugmichael`
- 接口业务分层：
    1. @ug_benjamin：主要负责：个人中心、登录、注册、投注、 等相关接口
    2. @ugmichael：主要负责：首页、购彩大厅、玩法详情、真人游戏、等相关接口

```
* 接口地址：http://test10.6yc.com/wjapp/api.php
    * 临时请求路径方式：?c=user&a=login，区别于传统路径：/user/login 
    * 注意：所有请求路径不要写死，方便后期转换
* 后台地址：http://admintest10.6yc.com
    * 管理员1：usr: admin, pwd: aa123456, code: 147258
* 前台地址：http://test10.6yc.com
```

## 接口文档

文档目前是采用 `swagger` + `postman` 结合的方式、`swagger`文档基于后端配置文件自动生成，集文档：请求说明、在线测试、编辑于一体，
唯一的缺点是`请求参数无法记忆`，而 `postman`弥补了`swagger`不能记忆请求参数的功能

* 请求说明文档 `swagger`
    * 向后端开发人员获取最新 `FH-API-DOC.json` 文件
    * 打开 swagger 在线编辑网页 (https://editor.swagger.io/)
    * 导入文件：[FILE -> Import FILE -> `FH-API-DOC.josn`]
    * 文档工具相关报错点击 [hide] 即可
    * swagger集接口：参数说明、请求模拟、在线编辑 于一体
    
* 测试用例文档 `postman`
    * 向后端开发人员获取最新 `FH-API-POSTMAN.json` 文件
    * 谷歌浏览器安装：`postman`、并导入`FH-API-POSTMAN.json`
    * [POSTMAN - 系列使用教程](https://blog.csdn.net/u013613428/article/details/51557804)

## 响应解析

```
// 响应格式一：APP格式（所有数据展示在data里）
  {
    "code": 0,
    "msg": '您已注册成功！',
    
    // data数据为空时的响应
    "data": null,
    
    // data单条数组时的响应
    "data": {
      "uid": 1,
      "usr": "12154545",
    }
    
    // data分页数据的响应
    "data": [
        "list" : [{"uid": 1,"usr": "asdasda"}, {"uid": 2,"usr": "xxxx"}],
        "total": 2
    ]
  }

// 响应格式二：普通格式 (额外参数：全在extra里，data要么是单条对象，要么是多条数组)
  {
    "code": 0,
    "msg": '您已注册成功！',
    
    // data数据为空时的响应
    "data": null,
    
    // data单条数组时的响应
    "data": {
      "uid": 1,
      "usr": "12154545",
    },
    
    // data分页或多条数据的响应
    "data": [
        {"uid": 1,"usr": "asdasda"}, 
        {"uid": 2,"usr": "xxxx"}
    ],
    
    // 额外响应：如：总条数、总金额
    "extra": {
      "token": 'xxxxx',
      "total": 1000,
      "totalAmount": 1000,
      "other1": [],
      "other2": {},
    }
  }  
  
 // 响应字段解析
 {
    "code": 0,               // 响应状态码：0：成功，1：失败
    "msg": "您已注册成功！",  // 响应提示信息
    "data": [],              // 响应数据主体（列表：json数组）（单条：json对象）
    "extra": {},             // 额外响应数据（json对象，必须带键名，键值可以是: 普通数据，json对象，json数组）
    "code|httPCode": 401,   // 未登录
    "code|httpCode": 403,   // 已登录、但没有权限访问
    "code|httpCode": 404,   // 访问资源不存在
    "code|httpCode": 500,   // 服务器异常
    "code|httpCode": 504,   // 服务器响应超时
  }


```

## 参数加密

参数加密目前基于：`3des` + `rsa`，所有参数名: `usr` key不变，参数值用：`3des('xiaoming', 3desKey)` 加密，

然后增加签名参数：`sign = rsa_pub(3desKey)`，用服务器提供的公钥， 对3desKey进行加密

```DEMO
请求路径：
?c=user&a=login

POST参数:  
usr=3des('xiaoming', 3desKey)
pwd=3des(md5('123456'), 3desKey)
sign=rsa_pub(3desKey) 

```

## 注意事项

* `请求路径不要写死`：目前服务器支持3层路径，?m=api&c=user&a=login，临时使用这种路径方式，
如果API项目完全独立后，会转为RESTFUL的路径请求方式：例 /api/user/login
* `参数加密不要写死`：可以通过开关配置
* `预留参数`：版本号`v`、 设备：`device`
* `服务器响应格式`: 你们自由选择，目前服务器通过ua解析、根据不同设备展示不同格式，但有不稳定性，依赖设备参数`device`比较靠谱