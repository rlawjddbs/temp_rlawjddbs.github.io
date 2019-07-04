---
title: PHP의 Autoload
updated: 2019-07-04 08:53
category: PHP
---
[참고 문서](https://freehoon.tistory.com/75?category=708152)
  
### PHP의 클래스 작성법
```php
# Hello.php
<?php
class Hello
{
    public function say()
    {
        echo "Hello- PHP!";
    } // say
} // class
```

### Hello 클래스의 인스턴스 생성 후 자원 사용
```php
# index.php
<?php
require_once 'Hello.php';

$sayHello = new Hello();
$sayHello->say();
```
require_once는 외부 파일을 가져오는 함수이며 공통적으로 사용할 코드는 이런식으로 별도의 php파일로 만든 후 각 파일에서 불러오는 것으로 코드의 양이 줄어들고, 수정이 용이해진다.

### Task01 클래스 작성
```php
# Task01.php
<?php
class Task01
{
    public function todoTask()
    {
        echo "todo Task!";
    }
}
```
새로운 클래스를 작성하였다. 해당 클래스의 인스턴스를 생성하기 위해서는 또 하나의 require_once 함수를 사용해야만 하는데 이러한 클래스가 계속해서 생겨 난다면 일일이 외부 파일을 불러오는 코드를 작성하는것은 비효율적이다.

### index.php 수정
```php
<?php
// require_once 'Hello.php';
// require_once 'Task01.php';
spl_autoload_register('my_autoloader');

$sayHello = new Hello();
$sayHello->say();

echo '<br>';

$task01 = new Task01();
$task01->todoTask();

function my_autoloader($class)

{
    require_once $class.'.php';
}
```

위 코드로 외부 파일을 불러와 손 쉽게 객체화를 할 수 있게 된다.
혹은 **익명함수** 방식으로도 사용할 수 있다.

### index.php 수정2
```php
<?php

spl_autoload_register(function ($class){
    require_once $class.'.php';
});

$sayHello = new Hello();
$sayHello->say();

echo '<br>';

$task01 = new Task01();
$task01->todoTask();
```