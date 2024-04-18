# 模型

## 文件声明
```php
<?php

/**
 * 玩家层级表
 */

namespace Api\Model;

use Api\Core\Mvc\Model;             // 模型基类

class Level extends Model
{
    const TABLE_PK   = 'id';        // 声明表主键（必须）
    const TABLE_NAME = 'level';     // 声明表名称（必须）

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

## 查询
* 单条数据查询

```php
// 根据主键查询
$findById = Members::findByPk($params['id'], 'uid, username, type, name, host');

// 根据某个字段查询
$findByUsr = Members::findOne([ 'username' => $params['usr'] ], 'uid, username, name');

// 原生SQL查询
$findBySql = Members::findBySql('SELECT * FROM `ssc_members` WHERE uid = :uid ', ['uid' => 1]]);
```

* 列表数据查询

```php
// 普通数据列表：data为数组，列入了要查询的字段和值，适用于简单左右相等的列表获取
$loginList = MemberSession::getDataList([
    'columns' => 'username',
    'data'    => [ 'loginIP' => $loginIp ],
]);

// 普通数据列表：处理in查询
$loginList = MemberSession::getDataList([
    'columns' => 'uid,username',
    'data'    => [ 'loginIP' => $loginIp, 'uid' => [1, 2, 3, 4] ],
]);

// 复杂查询列表：where为数组，能处理所有查询，但依靠条件拼装
$loginList = MemberSession::getDataList([
    'columns' => 'uid,username',
    'data'    => [ 'loginIP' => $loginIp ],
    'where'   => [ "accessTime > {$lastTime}", "username NOT LIKE 'guest_%'" ],
]);

// 带索引数据列表：声明 'arrKey', 值为索引的字段 
$loginList = MemberSession::getDataList([
    'columns' => 'uid,username',
    'data'    => [ 'loginIP' => $loginIp ],
    'where'   => [ "accessTime > {$lastTime}", "username NOT LIKE 'guest_%'" ],
    'arrKey'  => 'username'
]);

// 分页数据列表: 需要传递：page + rows 参数，有默认值，底层已自动获取适配
$loginList = MemberSession::getPageList([
    'columns' => 'uid,username',
    'data'    => [ 'loginIP' => $loginIp ],
    'where'   => [ "accessTime > {$lastTime}", "username NOT LIKE 'guest_%'" ],
    'arrKey'  => 'username'
]);

```

* 获取数据并生成缓存

```php
// 单条数据缓存
$game = Type::getOneByCache([ 'data' => [ 'id' => $gameId ], 'columns' => Type::$COLUMNS['bet'] ], 120);

// 列表数据缓存
$methods = Played::getListByCache([
    'data'   => [ 'type' => $gameId, 'name' => $name ],
    'arrKey' => 'id'
], 10);

```


## 新增

## 更新

## 删除

## 读写锁

## 增减量更新

## 总更新条数