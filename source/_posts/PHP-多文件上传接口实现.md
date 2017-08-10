---
title: PHP-多文件上传接口实现
date: 2017-08-10 21:46:06
tags: 
	- PHP 
categories: PHP
---

想测试多文件上传功能；

使用PHP接口模拟一个接口非常方便；

PHP多文件上传接收接口示例：

<!-- more -->

```php
<?php 
    //忽略警告
    error_reporting(E_ALL || ~E_NOTICE);
    //设置编码
    header("Content-type: text/html; charset=utf-8");
    /*设置时区*/
    date_default_timezone_set('prc');
  
    saveFiles();
  
    $isError = false;
  
    function saveFiles() {
        foreach ($_FILES as $name => $values) {
            saveFile($name);
        }
        if ($isError == false) {
            echo '{"code":"successfully", "data":{"msg":"上传成功！"}}';
        }
    }
  
    function saveFile($file)
    {
        $allowedExts = array("gif", "jpeg", "jpg", "png");
        $temp = explode(".", $_FILES[$file]["name"]);
        $extension = end($temp);
        if ((($_FILES[$file]["type"] == "image/gif")
        || ($_FILES[$file]["type"] == "image/jpeg")
        || ($_FILES[$file]["type"] == "image/jpg")
        || ($_FILES[$file]["type"] == "image/pjpeg")
        || ($_FILES[$file]["type"] == "image/x-png")
        || ($_FILES[$file]["type"] == "image/png"))
        && in_array($extension, $allowedExts))
        {
            if ($_FILES[$file]["error"] > 0)
            {
                $isError = true;
                echo '{"code":"fail", "data":{"msg":"上传失败！"}}';
                break;
            }
            else
            {

                if (file_exists("image/".$_FILES[$file]["name"]))
                {
                } else {
                    move_uploaded_file($_FILES[$file]["tmp_name"],
                    "image/".$_FILES[$file]["name"]);
                }
            }
        }
        else
        {
            $isError = true;
            echo '{"code":"fail", "data":{"msg":"非法文件格式！"}}';
            break;  
        }
    }
  
?>
```