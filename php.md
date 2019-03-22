

# php7 +

## null合并运算符 (??)
```php
<?php

$username = $_GET['user'] ?? 'nobody';
//这两个是等效的  当不存在user 则返回?? 后面的参数

$username = isset($_GET['user']) ? $_GET['user'] : 'nobody';

?>
```

## 
```php
// 整数
echo 1 <=> 1; // 0 当左边等于右边的时候，返回0
echo 1 <=> 2; // -1  当左边小于右边，返回-1
echo 2 <=> 1; // 1  当左边大于右边，返回1

// 浮点数
echo 1.5 <=> 1.5; // 0
echo 1.5 <=> 2.5; // -1
echo 2.5 <=> 1.5; // 1
 
// 字符串
echo "a" <=> "a"; // 0
echo "a" <=> "b"; // -1
echo "b" <=> "a"; // 1
```
## define
```php
define('ANIMALS', [
    'dog',
    'cat',
    'bird'
]);

echo ANIMALS[1]; // 输出 "cat"
```
allowField(true) 过滤不是数表中的字段
data([]) 方法批量赋值

strstr() 查找字符串，成功返回，失败返回false
strrpos() 成功大于-1
tp5 Route 中可用变量替换
多个需要可选判断的字体用数组循环