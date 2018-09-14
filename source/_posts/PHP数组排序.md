---
title: PHP数组排序
date: 2018-09-11 11:09:11
categories: PHP
tags: [PHP,数组，函数]
---
## PHP数组排序

### PHP - 数组的排序函数

- sort() - 以升序对数组排序
- rsort() - 以降序对数组排序
- asort() - 根据值，以升序对关联数组进行排序
- ksort() - 根据键，以升序对关联数组进行排序
- arsort() - 根据值，以降序对关联数组进行排序
- krsort() - 根据键，以降序对关联数组进行排序

#### 对数组进行升序排序 - sort()
下面的例子按照字母升序对数组 $cars 中的元素进行排序：

```
<?php
$cars=array("Volvo","BMW","SAAB");
sort($cars);

$clength=count($cars);
for($x=0;$x<$clength;$x++)
   {
   echo $cars[$x];
   echo "<br>";
   }
?>

结果：
BMW
SAAB
Volvo

```

下面的例子按照数字升序对数组 $numbers 中的元素进行排序：


```
<?php
$numbers=array(3,5,1,22,11);
sort($numbers);

$arrlength=count($numbers);
for($x=0;$x<$arrlength;$x++)
   {
   echo $numbers[$x];
   echo "<br>";
   }
?>

结果：
1
3
5
11
22

```
#### 对数组进行降序排序 - rsort()
下面的例子按照字母降序对数组 $cars 中的元素进行排序：
```
<?php
$cars=array("Volvo","BMW","SAAB");
rsort($cars);

$clength=count($cars);
for($x=0;$x<$clength;$x++)
   {
   echo $cars[$x];
   echo "<br>";
   }
?>

结果：
Volvo
SAAB
BMW

```
下面的例子按照数字降序对数组 $numbers 中的元素进行排序：
```
<?php
$numbers=array(3,5,1,22,11);
rsort($numbers);

$arrlength=count($numbers);
for($x=0;$x<$arrlength;$x++)
   {
   echo $numbers[$x];
   echo "<br>";
   }
?>


结果：
22
11
5
3
1

```

#### 根据值对数组进行升序排序 - asort()
下面的例子根据值对关联数组进行升序排序：
```

<?php
$age=array("Bill"=>"35","Steve"=>"37","Peter"=>"43");
asort($age);

foreach($age as $x=>$x_value)
    {
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
    }
?>

结果：
Key=Bill, Value=35
Key=Steve, Value=37
Key=Peter, Value=43

```

#### 根据键对数组进行升序排序 - ksort()
下面的例子根据键对关联数组进行升序排序：
```

<?php
$age=array("Bill"=>"35","Steve"=>"37","Peter"=>"43");
ksort($age);

foreach($age as $x=>$x_value)
    {
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
    }
?>

结果：
Key=Bill, Value=35
Key=Peter, Value=43
Key=Steve, Value=37

```

#### 根据值对数组进行降序排序 - arsort()
下面的例子根据值对关联数组进行降序排序：
```

<?php
$age=array("Bill"=>"35","Steve"=>"37","Peter"=>"43");
arsort($age);

foreach($age as $x=>$x_value)
    {
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
    }
?>

结果：
Key=Peter, Value=43
Key=Steve, Value=37
Key=Bill, Value=35

```

#### 根据键对数组进行降序排序 - krsort()
下面的例子根据键对关联数组进行降序排序：
```

<?php
$age=array("Bill"=>"35","Steve"=>"37","Peter"=>"43");
krsort($age);

foreach($age as $x=>$x_value)
    {
    echo "Key=" . $x . ", Value=" . $x_value;
    echo "<br>";
    }
?>

结果：
Key=Steve, Value=37
Key=Peter, Value=43
Key=Bill, Value=35

```
