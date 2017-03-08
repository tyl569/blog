---
title: PHP 零散函数（一）
date: 2016-09-23 16:00:36
tags:
---

#### parse_url

```php
mixed parse_url ( string $url [, int $component = -1 ] )

```
<br>
#### 作用：
> 解析一个URL并返回一个关联数组，包含URL各个组成部分

> *本函数不是用来验证给定 URL 的合法性的，只是将其分解为下面列出的部分。不完整的 URL 也被接受，parse_url() 会尝试尽量正确地将其解析。*

```php
$url = "http://3c.ycwb.com/2016-09/21/content_23077306.htm?name=tyler&pass=123";
$parse_array = parse_url($url);
echo "<pre>";
print_r($parse_array);
echo "</pre>";
//export:
//Array
//(
//    [scheme] => http
//    [host] => 3c.ycwb.com
//    [path] => /2016-09/21/content_23077306.htm
//    [query] => name=tyler&pass=123
//)
```

#### parse_str

```php
void parse_str ( string $str [, array &$arr ] )

```
<br>

#### 作用：
> 如果 str 是 URL 传递入的查询字符串（query string），则将它解析为变量并设置到当前作用域

> 如果设置了第二个变量 arr，变量将会以数组元素的形式存入到这个数组，作为替代。

```php
$str = "first=value&arr[]=foo+bar&arr[]=baz";
parse_str($str);
echo $first;  // value
echo $arr[0]; // foo bar
echo $arr[1]; // baz

parse_str($str, $output);
echo $output['first'];  // value
echo $output['arr'][0]; // foo bar
echo $output['arr'][1]; // baz

```