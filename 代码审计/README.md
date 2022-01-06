[toc]



# 代码审计

## 目录

### 简单

-   [HCTF 2018] WarmUp 难度：简单 知识点：初步的代码审计和文件包含
-   [BJDCTF2020]Mark loves cat 简单的代码审计，变量覆盖

### 签到

-   [HCTF 2018]admin 中等难度的题目，解法较多，分别有jwt伪造，条件竞争和unicode欺骗
-   [ZJCTF 2019]NiZhuanSiWe 基础的代码审计，解法较多，php伪协议
-   [BJDCTF2020]EasySearch 除了注入以外还会有Apache SSI 远程命令执行漏洞
-   [HarekazeCTF2019]encode_and_encode 编码绕过
-   [SUCTF 2019]EasyWeb 当年比较难的题目，现在这些考点被干碎了再出现就只能做签到了
-   [安洵杯 2019]不是文件上传 介乎于签到和中等之间，多个考点重合在一起了
-   [N1CTF 2018]eating_cms
-   [PASECA2019] honey_shop 读取环境变量，介乎于签到和中等之间
-   Phuck2
-   [网鼎杯 2020 总决赛]Game Exp
-   

### 中等

-   [De1CTF 2019]SSRF Me Flask 字符串拼接带来的安全问题
-   [HFCTF2020]EasyLogin jwt伪造
-   [SCTF2019]Flag Shop ruby 代码审计
-   [DDCTF 2019]homebrew event loop 逻辑漏洞
-   [XDCTF 2015]filemanager
-   [PwnThyBytes 2019]Baby_SQL
-   [SWPUCTF 2016]Web blogsys 哈希拓展攻击，逻辑漏洞
-   [PWNHUB 公开赛 2018]傻 fufu 的工作日 加解密逻辑最好自己能够掌握。
-   [CISCN2019 东北赛区 Day2 Web3]Point System
-   [HBCTF2017]大美西安
-   [N1CTF 2018]easy_harder_php soap_ssrf 非常经典的题目
-   [Zer0pts2020]notepad python反序列化

### 困难

-   [网鼎杯 2020 半决赛]faka 中等偏难
-   [RoarCTF 2019]PHPShe
-   [护网杯 2018]easy_laravel
-   [HMBCTF 2021]EzLight 红帽杯就3解还是几解我记得。
-   [HITCON 2017]Baby^h Master PHP Apache-prefork模型(默认模型)在接受请求后会如何处理,首先Apache会默认生成5个child server去等待用户连接, 默认最高可生成256个child server, 这时候如果用户大量请求, Apache就会在处理完MaxRequestsPerChild个tcp连接后kill掉这个进程,开启一个新进程处理请求
-   [CISCN2019 总决赛 Day2 Web2]Laravel File Manager 参考文章：https://blog.szfszf.top/article/39/ 也是国赛经典不让人做
-   Real World CTF 2018 Bookhub



### 脑洞

-   [羊城杯 2020]Blackcat 要听歌的WEB题目== 2020🐏城杯的题目感觉..

## Writeup

### [HCTF 2018] WarmUp

提示source.php

```php
<?php
    highlight_file(__FILE__);
    class emmm
    {
        public static function checkFile(&$page)
        {
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            if (! isset($page) || !is_string($page)) {
                echo "you can't see it";
                return false;
            }

            if (in_array($page, $whitelist)) {
                return true;
            }

            $_page = mb_substr(
                $page,
                0,
                mb_strpos($page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }

            $_page = urldecode($page);
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }

    if (! empty($_REQUEST['file'])
        && is_string($_REQUEST['file'])
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
?>
```

接收file，不为空，是字符串，满足emmm::checkFile

flag在ffffllllaaaagggg