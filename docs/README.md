# 快速开始
---

本文将从实例的角度，一步步地搭建出一个接口应用，让你能快速的入门

## 环境准备

- 操作系统：支持 Linux，Windows
- 运行环境：支持 5.4 - 7.2 版本 (为兼容服务器环境：目前只写PHP5的语法)
- 基础组件：NGINX, MYSQL, REDIS, 
- 代码仓库：
    * 代码主分支：api-201905，可以各自新建个分支，在各自测试机上测试、再在主分支上合并
    * GIT地址：https://git.fhptbet.com/root/fhptweb/tree/api-201905/web/wjapp
    * 测试服务器：
        1. www：http://test10.6yc.com
        2. mobile：http://test10.6yc.com/dist
        3. api：http://test10.6yc.com/wjapp/api.php
    

## 协作开发  
* 项目管理与协作开发
    * 代码管理：
        * 版本管理：GIT, 
        * 项目管理：禅道
        * IDE工具：phpstorm
    * 接口文档：
        * 自动生成：/wjapp/api.php?m=test&c=test&a=apidoc（会保存在：Docs/swagger/FH-API-DOC.json, 注意目录权限）
        * 接口测试：
            1. 官方服务：https://editor.swagger.io/ , 导入：FH-API-DOC.json
    
* 代码风格
    1. 代码格式化配置
        1. PHP风格：/docs/codeStyle/FH-PHP-STYLE.xml
        2. IDE导入：PHPSTORM：setting->codeStyle->php->import xml
        3. 代码：编码：UTF-8，换行：LF
    3. 代码格式化
        1. 文件：快捷键 ctrl + alt + l
        2. 目录：点击目录 + 快捷键
    4. PHP代码注释（必须带描述信息和作者）
        1. 接口必须带版本号，第二版有变动，需备注变动内容在版本号后

## 开发及建议
1. 能设计的简单，尽量设计简单，不能设计简单，把代码书写美观、注释清晰、分类合理、职责单一
2. 符合开闭原则：尽量对扩展开放、对修改封闭，功能方法尽量做成单一入口，
后期只维护这个操作入口，而不是全局搜关键词
3. 变量目录命名：语义化、专业 （清晰为主、简单为辅助）
3. 尽量区别当前业务或功能，是否为通用，不要有太多重复的代码
4. 备注要简明清晰，不要让其它同事重复走你走过的坑，复杂业务必须备注实现思路，要条理清晰
5. 代码应具有一定的工业美感，软件开发不像传统的商业产品，可以从外观、包装上来识别，更多的是需要软件开发人员自我约束
6. 站在更高的角度、设想更复杂的场景，如何处理和应对
   1. 堆成山的、不看文档不知要做什么的目录和文件、
   2. 命名不知其意，如果不看程序代码和备注
   3. 代码无法复用，相同代码复制了无数份
   4. 程序BUG频出， 如何尽量避免和快速排错  


## 业务|服务|工具 分层
1. 参数层：获取某个资源的请求方式、参数规则、以及相关属性的配置
2. 权限层：对访问的 IP、归属地、访问频率、资源有效性、是否公共服务、需要授权访问 等进行检查
3. 调度层：控制器层，业务调度层，可以是某个资源(model)、也可以是某个服务(service)
4. 模型层：业务层：具体某个资源的增删改查操作
5. 服务层：综合服务层：如登录、注册、投注 这种对：多个资源和数据调度以及复杂业务的处理及统一管理
5. 工具层：常用助手类：如加解密、数组处理、文件处理、字符串处理、数据过滤及验证

## [目录结构说明](fh-api/dir-detail.md)

## 编写接口
```php
'user_login'          => [
        'title'         => '用户登录',
        'tags'          => 'user',
        'http_method'   => 'POST',
        'is_free'       => true,
        'data_rules'    => [
            'usr'   => [ 'check' => 'isUsr', 'min' => 6, 'max' => 15, 'msgPrefix' => '用户名' ],
            'pwd'   => [ 'check' => 'isPwd', 'len' => 32, 'msgPrefix' => '用户密码' ],
        ],
        'request_limit' => [
            'limitType'  => 'IP',
            'limitCycle' => 1,
            'limitNum'   => 1,
        ],
        'return'        => [
            'uid'   => 101,
            'usr'   => 'test',
            'token' => md5('test')
        ]
    ],
```

## 编写数据验证规则

```php
'data_rules'  => [
    'token'      => [ 'check' => 'isHash', 'min' => 24, 'max' => 64, 'msgPrefix' => '授权TOKEN' ],
    'gameId'     => [ 'check' => 'isInt', 'than' => 0, 'msgPrefix' => '游戏ID' ],
    'betIssue'   => [ 'check' => 'isInt', 'msgPrefix' => '投注期号' ],
    'endTime'    => [ 'check' => 'isTimestamp', 'msgPrefix' => '封盘时间' ],
    'totalNum'   => [ 'check' => 'isInt', 'than' => 0, 'lessThan' => 1000, 'msgPrefix' => '投注总组数' ],
    'totalMoney' => [ 'check' => 'isAmount', 'isInt' => true, 'than' => 0, 'msgPrefix' => '投注总金额' ],
    'betBean'    => [ 'check' => 'isArray', 'msgPrefix' => '投注详情' ],
    ],
```

## 编写控制器

```php
<?php
/**
 * 默认控制器
 */

use Api\Core\Mvc\Controller;
use Api\Utils\Validator;
use Api\Core\Exception\ErrMsg;

class IndexController extends Controller
{
    /**
     * @title  默认方法
     * @since  1.0.0
     * @author benjamin
     */
    public function index () {
        $this->success('welcome to fh-api 1.0...');
    }

}
```

## 编写模型
```php
<?php

/**
 * 玩家层级表
 */

namespace Api\Model;

use Api\Core\Mvc\Model;

class Level extends Model
{
    const TABLE_PK   = 'id';
    const TABLE_NAME = 'level';

    /**
     * @title 根据ID获取层级信息
     * @param $id
     * @return mixed
     * @author benjamin
     */
    public static function getLevelById ( $id ) {
        return self::findByPk($id);
    }

}

```

## 常用功能

* 请求数据数组：$this->getSafeData();  (通用：get/post/put/delete，会过滤api.php未声明的字段)
    * 请求数据单条：$this->getSafeDataByKey($k, $def); (通用：get/post/put/delete，会过滤api.php未声明的字段)
    * 客户端IP地址：$this->IP();
    * 客户端IP地区：$this->IpLocation($ip);
    * 请求相关参数：$this->request->getXXX(),  // 取自：$_SERVER，每个方法有对应注释和示例
    * 登录相关：
        * 登录信息：$this->session->authUser['xxx']
        * 是否登录：$this->hasLogin();
    * 网站配置：  // 所有配置：包括数据库和文件相关配置，新增配置需要和负责人商量
        1. 主配置文件：$this->settings['app']['xxx']  // mysql,redis,staticServer 等核心配置
            1. 环境配置文件：/customise/config/config.api.env.php, 会覆盖app.php配置
        2. 接口配置：$this->settings['api']['xxx']    // 所有接口配置
        3. 网站配置：$this->settings['system']['xxx'] // params表所有配置
        4. 游戏配置：$this->settings['game']['xxx']   // 游戏相关配置
    * 数据库操作：看Model层相关示例    

## 编写服务