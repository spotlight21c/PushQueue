# PushQueue

# Introduction #

## 상태알림, 스마트폰 푸시알림으로 쉽게 받자 ##

시스템 관리자를 비롯한 개발자분들, 여러분의 시스템이 발송하는 상태 알림 어떻게 받아보고 있습니까?

시스템의 상태 모니터링이나 스케줄, 배치작업, 특정이벤트 발생 등의 알림을 어떻게 확인하고 계십니까?

**이메일?** 비용이 들지않아 좋습니다.
하지만 내가 일상적으로 받는 무수히 많은 이메일 속에서 이러한 메일은 파묻혀서 안봅니다.

**SMS?** 꼬박꼬박 확인한다는 점에서 좋으나, SMS모듈을 적용하려면 이메일발송 기능을 구현하는거보다 복잡합니다. 또한 비용도 청구되고요. 수시로 까먹지 않고 SMS 충전해주어야 합니다.

**모니터링앱 직접구축?** 발송에 비용이 들지 않고, 확인하기에 편합니다.
그러나 직접 앱을 개발하고, 서버 셋팅하여 apns와 gcm을 직접 구현해야되는점이 가장 큰 장벽입니다.
특히 apns 이용하려면 애플개발자프로그램에 가입해야되고, 매년 apns인증서도 갱신해줘야되고 신경써야할것들이 많습니다.

그래서 개발자분들이 사용하기 쉽도록 push 알림보내는 기능을 RESTful하게 만들었습니다. 다른건 신경쓰지마시고 어디든지 붙여서 쓰세요.

시스템 상태 부터 주문알림 등 무궁무진하게 활용가능합니다.

**무료로 제공하니 편하게 구현하여 이용하세요.**

# How to use #

먼저, 푸시큐를 설치합니다. [구글플레이](https://play.google.com/store/apps/details?id=com.trend21c.pushqueue), [앱스토어](https://itunes.apple.com/app/pushqueue/id892027180?mt=8&uo=4)

## 푸시알림 보내기 ##

1. 앱에서 그룹을 생성합니다.

2. 앱의 첫화면에서 UUID와 secret_key를 확인합니다.

3. 적절한 곳에서 API를 이용하여 호출합니다.

4. 푸시알림을 받을 사람들에게 그룹코드를 알려주고 등록하도록 요청합니다.

끝.

## 푸시알림 받기 ##

1. 앱에서 푸시알림받을 그룹코드를 등록합니다.

끝.

※ 웹으로도 발송할수 있습니다. http://push.doday.net/api/send_form

# API #

POST http://push.doday.net/api/push

## Parameters ##

|변수명|타입|설명|
|--:|---|---|
|uuid|string|앱에서 확인가능한 uuid|
|secret_key|string|앱에서 확인가능합 secret_key|
|code|string|푸시 보낼 타겟 그룹 코드(내가 개설한 그룹만 가능)|
|body|string|문자열은 자동으로 길이가 조절되어 발송됩니다.(utf8)|

## Example ##

### curl ###

```
#!curl

curl -d "uuid=my_uuid&secret_key=my_secret_key&code=target_group_code&body=your_message" http://push.doday.net/api/push
```

### php ###


```
#!php

$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, 'http://push.doday.net/api/push');
curl_setopt($ch, CURLOPT_POST, true);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, 'uuid=my_uuid&secret_key=my_secret_key&code=target_group_code&body=your_message');
$result = curl_exec($ch);
// $result_json_object = json_decode($result, true);
curl_close($ch);
```


# Errors #

에러코드 안내

## 대표 에러코드 ##

* 9001 : 일치하는 그룹없음
* 9002 : secret_key 불일치
* 9003 : 마지막으로 발송한 푸시가 아직 완료되지 않았음
* 9004 : 알수없는 오류(서버오류 등)

## Response ##

|변수명|타입|설명|
|--:|---|---|
|result|string|success 또는 fail|
|code|string|에러 발생시 에러코드|
|error_description|string|에러 설명|

## Example ##


```
#!json

{"result":"success"}
```


```
#!json

{"result":"fail", "code":"error code"}
```
