PHP Decrypt
=======
[![License](https://img.shields.io/badge/license-apache2-blue.svg)](LICENSE)

**PHP Decrypt是一个跨平台用来解密PHP源码的扩展，可以解密部分使用扩展跟eval加密的代码。**

## 特点

- 兼容 OPcache、Xdebug 等其他扩展
- 兼容 Linux、OSX、Windows 等系统
- 兼容 Apache、Nginx + php-fpm、命令行等运行模式

## 安装

### 环境要求

- Linux,OSX,Windows
- PHP7.0.0以上(暂不支持php5版本)

> ⚠ 安装前请把当前PHP环境的php-config添加到环境变量中。

### 在Linux,OSX上编译

```shell
git clone https://github.com/2654709623/php-decrypt.git
cd php-decrypt
phpize
./configure
make
sudo make install
```

### 在Windows上编译

可以自行参考本篇文章：https://learnku.com/articles/22323

### TODO

1.目前所有解密代码只能保存在一个文件中以文件名区分  

2.暂时还不能解密一些goto加密和混淆加密

## 使用

> 上面我们已经编译好PHPDecrypt扩展了，PHPDecrypt安装好默认是关闭自动解密功能的，需要我们在php.ini文件中来配置使用它。

### 1.非扩展方式的加密源码(eval加密源码)

在你的php.ini文件中加入以下配置项:

```shell
extension=你的扩展路径/decrypt.so
[decrypt]
;用来配置解密功能开关,默认为disable
decrypt.switch=enable
;解密后的php源码存放目录
decrypt.save_path=/www/decrypt
```

### 2.扩展方式的加密源码

由于PHP扩展加载顺序是栈加载的，需要在加密扩展前先加载解密扩展，php.ini配置项如下:

```shell
extension=你的扩展路径/decrypt.so
[decrypt]
;用来配置解密功能开关,默认为disable
decrypt.switch=enable
;解密后的php源码存放目录
decrypt.save_path=/www/decrypt
;假设这是你的加密扩展,你需要在它之前先加载解密扩展
;extension=你的加密扩展路径/encryption.so
```
保存php.ini文件并重启php-fpm，然后访问加密的源码文件，就会把解密的源码文件保存到/www/decrypt/decrypt.code的文件中。

如果不需要使用解密功能的话可以把php.ini文件中的decrypt.switch=enable设置为decrypt.switch=disable，重启php-fpm。

## License

Apache License Version 2.0 see http://www.apache.org/licenses/LICENSE-2.0.html