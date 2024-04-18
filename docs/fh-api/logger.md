# 文件日志

## 日志分类

```php
    const LEVEL_DEBUG = 'debug'; # 调试
    const LEVEL_INFO  = 'info';  # 普通
    const LEVEL_WARN  = 'warn';  # 警告
    const LEVEL_ERR   = 'error'; # 错误
```

## 操作示例

```php
    /**
     * @title  日志生成
     * @throws Exception
     * @author benjamin
     */
    public function logTest () {
        $log = [
            'data' => $_SERVER,
        ];

        # 正常调用
//        Logger::write($log['data']);
//        Logger::write($log['data'], '/info/system', Logger::LEVEL_DEBUG);

        Logger::write('究竟是怎么肥事', '/2019-08/server', Logger::LEVEL_DEBUG);

//        Logger::write($log['data'], '/2019-08/server', Logger::LEVEL_INFO);

//        # 异常调用：空数据
//        Logger::write('', 'info/test', 'xxx');
//        # 异常调用：未定义的日志等级
//        Logger::write('究竟是怎么肥事', 'info/test', 'xxx');
//        # 异常调用：错误的路径拼接：../index | pay//alipay
//        Logger::write('究竟是怎么肥事', 'info//test', Logger::LEVEL_ERR);

        $this->success('日志生成成功！', $log);
    }

```

## 日志内容