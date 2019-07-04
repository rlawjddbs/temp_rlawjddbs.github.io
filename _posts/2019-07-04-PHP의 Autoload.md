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
