# 调度层

## 控制器分层 
1. 接口多版本 （已预留后期调整）
2. 项目多应用 （已预留后期调整）

## 文件声明

只声明了控制器文件并不能成为一个接口，必须声明在接口配置文件里： `api.php` 

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

## 响应结果

* 成功的响应

```php

// 只响应消息
$this->success('welcome to fh-api 1.0...');

// 响应消息 + 主体数组
$this->success('success', $data);

// 响应消息 + 主体数组 + 额外数据
$this->response->setExtra('total', 10);
$this->response->setExtra('totalAmout', 1000);

$this->success('success', $data);

```
  

* 失败的响应

```php
$this->error('用户资料更新失败，可能服务器繁忙。请稍后重试');

```