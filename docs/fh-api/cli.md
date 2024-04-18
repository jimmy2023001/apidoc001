# 命令行

命令行入口文件在根目录下的：`Cli.php` + `Console.php`，这两个文件重新调度了服务器组件，并能复用所有资源

目前命令行支持的服务并不多，后续待完善

# 模型生成

```php
php cli.php -p 'console/createModel' --data 'member_log'

```
![](../_images/console-img.png)

