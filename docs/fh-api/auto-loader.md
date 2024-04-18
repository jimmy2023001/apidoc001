# 自动加载

## registerBasePath - 遵循PSR4规范：通过目录层层寻找

## registerNamespaces - 指定命名空间和文件目录

## registerDirs - 目录自动加载

## registerFiles - 文件自动加载

## register - 自动加载注册

## 使用示例
```php
    /**
     * @title  自动加载调度
     * @author benjamin
     */
    private function autoLoader () {
        include API_PATH . 'Core/Loader.php';
        $loader = new Loader();

        # 配置顶级命名空间（遵循PSR4规范：通过目录层层寻找）
        $loader->registerBasePath([
            'Api'   => ROOT_PATH . '/wjapp/',
            'Admin' => ROOT_PATH . '/admin/',
        ]);

        # 根据指定的命名空间和目录去加载
        # 引用的第三方包可使用此加载方式
        $loader->registerNamespaces([
            'Api\\Core'       => ROOT_PATH . '/wjapp/Core/',
            'Api\\Core\\Http' => ROOT_PATH . '/wjapp/Core/Http/',
            "Api\\Service"    => ROOT_PATH . '/wjapp/Service/',
            "Api\\Utils"      => ROOT_PATH . '/wjapp/Utils/',
        ]);

        # 引入文件
        $loader->registerFiles([
            ROOT_PATH . '/wjapp/Config/api.php',
            ROOT_PATH . '/wjapp/Config/game.php',
            ROOT_PATH . '/wjapp/Config/ban.php',
            ROOT_PATH . '/wjapp/Config/real.php',
            ROOT_PATH . '/wjapp/Config/ip.php',
        ]);

        # 注册自动加载
        $loader->register();

        self::$di['traceBack']['loader'] = runtime(null, microtime(true), true);
    }

```