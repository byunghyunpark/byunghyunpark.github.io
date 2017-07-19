---
layout: post
comments: true
categories: AWS Rabbitmq
tags: Django Celery
---

# AWS EC2 인스턴스에 RabbitMQ 서버 올리기

---

Django 인스턴스가 오토스케일링 되어서 Celery broker를 별도 EC2 인스턴스로 분리하였습니다.

Celery broker는 일반적으로 RabbitMQ를 사용하여 세팅했습니다.


## AWS EC2 인스턴스 생성

AWS에서 EC2를 하나 생성합니다. 작은 서비스라 일단 저렴한 마이크로 사이즈로 생성합니다.

인스턴스는 Quick Start에서 Ubuntu로 생성했습니다.

Step 6: Configure Security Group 단계에서 22, 5672, 15672, 25672 포트를 열어줍니다.

## RabbitMQ 설치

SSH로 EC2에 접근합니다. 그리고 아래 명령으로 rabbitmq를 설치합니다.

```
sudo apt-get -qy update
sudo apt-get -qy install rabbitmq-server
```

## 서버 호스트 매핑

/etc/hosts 경로로 접근해서 노드를 변경해줍니다.

```
ec2-11-111-11-111.ap-northeast-0.compute.amazonaws.com # 접속한 ec2의 Public DNS를 적으세요

# The following lines are desirable for IPv6 capable hosts
::1 ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
ff02::3 ip6-allhosts                                              
```


## 어드민 유저 생성

리모트 호스트(celery)가 접근할 수 있도록 유저를 새롭개 생성합니다.

```
sudo rabbitmqctl add_user <user> <password>
sudo rabbitmqctl set_user_tags <user> administrator
sudo rabbitmqctl set_permissions -p / <user> ".*" ".*" ".*"
```

기존 guest 유저는 삭제합니다.

```
sudo rabbitmqctl delete_user guest
sudo /etc/init.d/rabbitmq-server restart
```

이제 리모트 유저(celery)는 user:password 정보로 서버에 엑세스 할 수 있습니다.


## RabbitMQ management plugin 활성화

rabbitmq-management 플러그인은 RabbitMQ 서버 모니터링 및 관리 기능을 제공합니다.

```
sudo rabbitmq-plugins enable rabbitmq_management

sudo /etc/init.d/rabbitmq-server restart
```

이제 바로 Rabbitmq 서버를 모니터링 할 수 있습니다.

```http://<ec2-public-dns>:15672``` 경로로 접근해보세요.

짠!

![rabbitmq management](/assets/rabbitmq-management.png)

## 마지막 celery 연결

django root 모듈 settings.py 에 브로커 주소를 입력합니다.

```
BROKER_URL = amqp://user:password@<ec2-public-dns>//
```

자 이제 Rabbitmq 서버를 마음껏 사용해보세요
