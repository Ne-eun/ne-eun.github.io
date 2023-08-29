---
layout: default
title: AWS의 보안그룹
parent: DevOps
nav_order: 1
---

## AWS의 보안그룹
보안그룹이란 EC2인스턴스의 접근하는 ip나 인스턴스를 관리한다.   
보안그룹끼리의 합집합으로도 만들 수 있다.   

### 인바운드 규칙
해당 보안그룹을 할당받은 인스턴스에 접근 가능하도록 허용된 수신 트래픽을 제어한다.


### 아웃바운드 규칙
해당 보안그룹을 할당받은 인스턴스에서 내보내기가 가능한 허용된 발신 트래픽을 제어한다.

![Image Alt 텍스트]({{site.url}}/assets/imgs/security_group_1.png )
포트, ip 등을 지정할 수 있고 다른 보안그룹을 할당할 수도 있다.

## 다른 보안그룹을 추가 할 때
A라는 보안그룹에 B라는 보안그룹을 넣는다면
![Image Alt 텍스트]({{site.url}}/assets/imgs/security_group_2.png )
A를 할당받은 인스턴스는 B를 할당받은 인스턴스가 HTTP 80으로 접근 가능하다는 의미이다.   
헷갈리지 말자.   