# 运行环境

一个 Web 应用本身应该是无状态的，并拥有根据运行环境设置自身的能力。

## 配置文件说明

* 核心配置文件在 /wjapp/Config/app.php
* 环境配置文件在 /customise/config/config.api.env.php [覆盖核心配置文件]
* 如果不配置环境配置文件，默认配置都是以正式环境为主
* 核心环境参数：
    * apiMode：正式与开发环境的主要区别是，开发环境会直接将错误暴露
    * pathInfo：解析请求路径的不同方式：参数、路径、nginx配置

## 环境配置参数
```php
'pathInfo'     => 'paramsCA',   // 路径解析：通过ca参数获取
'fileConfMode' => 'old',        // 配置文件：old：使用旧配置，new：使用新配置
'apiMode'      => 1,            // 开发模式：1:开发 2：测试 3：正式
'ipVisit'      => true,         // IP访问：true: 允许，false: 拒绝
'checkSign'    => false,        // 签名验证：是否开启参数加密与验签
'checkCountry' => true,         // 所属国家：归属地可访问性检查

```

## 配置完整文件

```php
<?php
/**
 * 环境配置文件：覆盖 /config/app.php
 */

include_once ROOT_PATH . '/customise/config/config.database.php';
include_once ROOT_PATH . '/customise/config/config.redis.php';
include_once API_CONFIG . '/func.php';

# MYSQL 主|写 节点
$conf['db']['name'] = $dbname;
$conf['db']['host'] = $dbhost;

# 适配MYSQL旧文件配置
$dbMasterList = getDbListFunc('master', $conf);
$dbSlaveList  = getDbListFunc('slave', $conf);

# 适配REDIS旧文件配置
$redisClusterList = getRedisListFund('cluster', $conf);
$redisServerList  = getRedisListFund('server', $conf);


$config = [
    'title'        => '凤凰APP接口 1.0',
    # 框架相关配置
    'project'      => [
        'version'      => '1.0.0',
        'pathInfo'     => 'paramsCA',   // 路径解析：通过ca参数获取
//        'pathInfo'     => 'requestUrl',   // 路径解析：通过$_SERVER['requestUrl']获取
        'fileConfMode' => 'old',        // 配置文件：old：使用旧配置，new：使用新配置
        'apiMode'      => 1,            // 开发模式：1:开发 2：测试 3：正式
        'ipVisit'      => true,         // IP访问：true: 允许，false: 拒绝
        'checkSign'    => false,        // 签名验证：是否开启参数加密与验签
        'checkCountry' => true,         // 所属国家：归属地可访问性检查
    ],
    # mysql-服务节点配置
    'mysql'        => [
        # 主节点集
        'masterList'    => $dbMasterList,
        # 从节点集
        'slaveList'     => $dbSlaveList,
        # 多连接模式：true：从节点配置里随机选择，false: 复用已连接上的对应节点
        'multiConnMode' => false,
        # 其它信息使用主库
        "usr"           => $conf['db']['user'],
        "pwd"           => $conf['db']['password'],
        "db"            => $conf['db']['name'],
        "charset"       => $conf['db']['charset'],
        'prefix'        => $conf['db']['prename'],
        'debug'         => true,
        'options'       => [],
        'expire'        => 3, // 连接超时：3秒
    ],
    # redis-服务节点配置
    'redis'        => [
        # 集群配置（优先使用）
        'clusterList' => $redisClusterList,
        # 普通节点配置
        'serverList'  => $redisServerList,
        # 其它信息使用主库
        'timeout'     => 3,
        'pwd'         => '',
        'persistent'  => false,  // 长连接: 待测试
        'prefix'      => $conf['db']['name'],
        'db_id'       => '0',
    ],
    # 缓存过期时间配置(秒)
    'cache'        => [
        'settings'     => 10, // 全局设定缓存时间
        'requestLimit' => 300, // 请求限制缓存时间
    ],
    # 头像路径
    'avatarPath'   => '/images/face/memberFace',
    # 文件日志配置
    'log'          => [
        'drive'         => 'file',
        'needSaveTypes' => [ 'ErrMsg', 'ErrParams', 'ErrHttp', 'ErrSign', 'ErrRedis', 'PDOException', 'RedisException', 'Exception', 'Throwable' ],
        'register'      => true,
        'login'         => true,
    ],
    # 全站接口限制
    'requestLimit' => [
        'limitType'  => 'IP',
        'limitCycle' => 0, // 每多少秒
        'limitNum'   => 0, // 可以请求多少次
    ],
];

$GLOBALS['APP_ENV_CONFIG'] = $config;



```