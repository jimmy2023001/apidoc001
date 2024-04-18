###### 在快速入门中，大家对框架应该有了初步的印象，接下来我们简单了解下目录约定规范。

```
API-DIR
├── Common
├── Config
|   ├── api.php
|   ├── app.php
│   ├── func.php
├── Controller
├── Core
|   ├── Exception
│   ├── Http
|   ├── Mvc
│   ├── Console.php
│   ├── db.php
│   ├── loader.php
|   ├── Permisson.php
│   ├── Plugin.php
|   ├── Redis.php
│   ├── Router.php
│   ├── Session.php
├── Docs
|   ├── code-style
|   ├── crypt
|   ├── demo
|   ├── env
|   ├── mysql
|   ├── nginx
|   ├── redis
│   ├── swagger 
├── Lib
├── Model
├── Public
|   ├── admin
│   ├── api
│   ├── cli
|   ├── pay
│   ├── service
│   └── www
├── Model
├── Runtime
|   ├── Cache
|   ├── Logs
├── Service
|   ├── Cache
│   ├── Logs
├   ├── Logs
├── Utils
|   ├── Arr
│   ├── File
│   ├── Hash
|   ├── str
│   ├── Enum
│   ├── Date
|   ├── Validator.php
|   ├── Filter.php
├── api.php
├── boot.php
├── api.php
├── Console.php        
```        

* Config: 配置文件目录，主要放接口相关配置和其它配置
    * api.php 所有接口基于此文件配置
    * app.php 项目核心配置
* Core: 核心组件层, 如: redis, session, router
* Lib: 第三方类库, 如: aliyun, oauth,
* Controller: 业务调度层：负责调度服务层、数据层，并响应输出
   * 控制器路径解析：
        * 默认：api.fh.com/({默认控制器：可配置}/{默认方法：可配置})
        * 一层：api.fh.com/{默认控制器：可配置}/a
        * 二层：api.fh.com/c/a
        * 三层：api.fh.com/m/c/a (如果带Module层，控制器文件上层目录为：Module名称，如Test)
* Models: 数据资源层：主要处理资源增删改查
* Service: 综合服务层：如登录、注册、投注 这种对：多个资源和数据调度以及复杂业务的处理
* Utils: 常用助手类：如加解密、数组处理、文件处理、字符串处理
* Docs: 项目以及接口相关文档
* Runtime: 运行时的临时文件

* Common （公共服务：后续完善）
     * Yaconf: 前后台共用配置 (配置文件 - php扩展)
     * Swoole: 前后台共用服务（RPC调度中心，耗时任务的处理）
     * Utils:  通用工具类库  （1: 写PHP扩展，2：通过目录架构共用）
     * Command:命令行操作支持（框架脚手架，定时任务）
     * Events：事件中间件层 （组件解耦）
     * Lang:   多语言支持 (独立支付系统中启用)