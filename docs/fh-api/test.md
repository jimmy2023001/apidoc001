# 测试

单元测试对项目稳定性以及正确性的作用不言而喻，

但目前并未使用相关断言框架，而是模拟请求调度、以及不同的条件去检查程序的正确性

控制器层支持模块分层，测试所有文件放在: /Controller/Test

## 模型所有操作测试： /Test/ModelController.php

## REDIS相关操作测试： /Test/RedisController.php

## 工具脚手架测试： /Test/UtilsController.php

## 浏览器识别测试： /Test/BrowserController.php

## 异常信息以及错误级别模拟： /Test/ErrorController.php