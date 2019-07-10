---
title: Godomall5 parameter 처리
updated: 2019-07-10 15:37
category: PHP
---
<a href="http://doc.godomall5.godomall.com/Godomall5_Pro_Guide/Coding_Guide#page_Superglobals" target="_new">참고문서</a>  
**NOTE** : 고도몰5 솔루션의 파라미터 처리

> 고도몰5 솔루션은 PHP 기반이지만 **$_GET, $_POST**와 같은 Superglobals 변수를 unset 하였으며, 각 변수의 역할을
대체하는 클래스와 함수를 제공한다.

```php
/* 고도몰에서 대체한 전역 변수들 */
$GLOBALS = Framework\Registry\Globals;
$_SERVER = \Request::server();
$_GET = \Request::get();
$_POST = \Request::post();
$_FILES = \Request::files();
$_REQUEST = \Request::request();
$_SESSION = \Session;
$_COOKIE = \Cookie;
```
따라서 기존 PHP에서 사용되는 전역변수를 이용하여 parameter 접근시 아무값도 가져오지 못한다.  
요청처리 방식이 GET이거나 POST일 경우 하기에 작성된 문법을 따른다.

```php
$_GET = \Request::get()->get('parameter명');
$_POST = \Request::post()->get('parameter명');
```
