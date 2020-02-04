# laravel5.6安装配置
服务器要求
- **Laravel 框架对PHP版本和扩展有一定要求，不过这些要求 Laravel Homestead 都已经满足了，不过如果你没有使用 Homestead 的话（那真是一件很遗憾的事情），有必要了解下这些以便确认自己的环境满足要求：**
- PHP >= 7.1.3
- PHP OpenSSL 扩展
- PHP PDO 扩展
- PHP Mbstring 扩展
- PHP Tokenizer 扩展
- PHP XML 扩展
- PHP Ctype 扩展
- PHP JSON 扩展
满足以上需求之后，就可以开始安装 Laravel 了。

# 配置 Laravel
初始化配置
公共目录
安装完 Laravel 后，需要将 Web 服务器的 document/web 根目录指向 Laravel 应用的 public 目录，该目录下的 index.php 文件作为前端控制器（单一入口），所有 HTTP 请求都会通过该文件进入应用。

配置文件

Laravel 框架的所有配置文件都存放在 config 目录下，所有的配置项都有注释，所以你可以轻松遍览这些配置文件以便熟悉所有配置项。

目录权限

安装完 Laravel 后，需要配置一些目录的读写权限：storage 和 bootstrap/cache 目录对 Web 服务器指定的用户而言应该是可写的，否则 Laravel 应用将不能正常运行。如果你使用 Homestead 虚拟机做为开发环境，这些权限已经设置好了。

应用key

接下来要做的事情就是将应用的 key（APP_KEY）设置为一个随机字符串，如果你是通过 Composer 或者 Laravel 安装器安装的话，该 key 的值已经通过 php artisan key:generate 命令生成好了。

通常，该字符串应该是 32 位长，通过 .env 文件中的 APP_KEY 进行配置，如果你还没有将 .env.example 文件重命名为 .env，现在立即这样做。如果应用 key 没有被设置，用户 Session 和其它加密数据将会有安全隐患!

Laravel 几乎不再需要其它任何配置就可以正常使用了，不过，你最好再看看 config/app.php 文件，其中包含了一些基于应用可能需要进行改变的配置，比如 timezone 和 locale（分别用于配置时区和本地化）。

你可能还想要配置 Laravel 的一些其它组件，比如缓存、数据库、Session 等，关于这些我们将会在后续文档一一探讨。

# Web 服务器配置
关于虚拟主机的配置（映射域名到Laravel应用目录）略过，如果了解细节可参考这篇教程，当然也可以留待下一篇讲 Homestead 和 Valet 再去了解。本文只探讨如何美化 URL 让其更具有可读性。
Apache

框架中自带的 public/.htaccess 文件支持隐藏 URL 中的 index.php，如过你的 Laravel 应用使用 Apache 作为服务器，需要先确保 Apache 启用了mod_rewrite 模块以支持 .htaccess 解析。

如果 Laravel 自带的 .htaccess 文件不起作用，试试将其中内容做如下替换：

Options +FollowSymLinks

RewriteEngine On

RewriteCond %{REQUEST_FILENAME} !-d

RewriteCond %{REQUEST_FILENAME} !-f

RewriteRule ^ index.php [L]

Nginx
如果你使用的是 Nginx，使用如下站点配置指令就可以支持 URL 美化：

  location / {

    try_files $uri $uri/ /index.php?$query_string;
    
  }

  
当然，使用 Homestead 或 Valet 的话，以上配置已经为你配置好，无需额外操作。
