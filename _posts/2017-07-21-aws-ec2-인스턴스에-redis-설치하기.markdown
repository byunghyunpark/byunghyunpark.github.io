---
layout: post
comments: true
categories: AWS Redis
tags: AWS EC2 Redis Cache
---

> 저는 Read cache를 위해  별도 EC2 인스턴스를 띄우고 redis를 사용하였습니다.

<br>

# EC2 create 

- aws amazone linux AMI를 선택합니다.
- security group 에 16379, 6379 포트를 추가합니다.
- 인스턴스 생성완료

<br>

# Linux updates
> ssh로 접근해서 업데이트를 실행합니다.

```
$ sudo yum -y update
$ sudo yum -y install gcc make
```

<br>

# Download Redis
> 참고: http://redis.io/download

```
$ cd /tmp
$ wget http://download.redis.io/releases/redis-4.0.0.tar.gz
$ tar xzf redis-4.0.0.tar.gz
$ cd redis-4.0.0
$ make
```

<br>

# Create Directories and Copy Redis Files

```
$ sudo mkdir /etc/redis 
$ sudo mkdir /var/lib/redis
$ sudo cp src/redis-server src/redis-cli /usr/local/bin/
$ sudo cp redis.conf /etc/redis/
```

<br> 

# Configure Redis.Conf

> conf 설정 파일을 수정합니다.

```
$ sudo vim /etc/redis/redis.conf

```
> 아래 내용 외 나머지 부분은 기본값을 유지합니다.

```
[..]
daemonize yes
[..]
  
[..]
bind 0.0.0.0
[..]
  
[..]
dir /var/lib/redis
[..]
  
logfile /var/log/redis_6379.log
```

<br>

# Setting Redis-Server init script

> 참고: https://github.com/saxenap/install-redis-amazon-linux-centos

- 자동 실행을 위한 스크립트를 다운 받습니다. 

	```
	$ cd /tmp
	$ wget https://raw.github.com/saxenap/install-redis-amazon-linux-centos/master/redis-server
	```

- Redis-Server 파일 이동 후 권한을 설정합니다.

	```
	$ sudo mv redis-server /etc/init.d
	$ sudo chmod 755 /etc/init.d/redis-server
	$ sudo vim /etc/init.d/redis-server
	
	->  redis="/usr/local/bin/redis-server" 확인
	```

- Redis-Server Auto-Enable 설정

	```
	$ sudo chkconfig --add redis-server
	$ sudo chkconfig --level 345 redis-server on
	```

<br>

# Start Redis Server
> 드디어 서버를 실행합니다.

```
$ sudo service redis-server start
$ redis-cli ping -> PONG

강제 종료시
$sudo service redis-server stop
```
