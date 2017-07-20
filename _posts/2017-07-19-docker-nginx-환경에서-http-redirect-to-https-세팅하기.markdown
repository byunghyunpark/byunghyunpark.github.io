---
layout: post
comments: true
categories: AWS Nginx Docker
tags: Django Nginx Docker ECS AWS
---

# docker nginx 환경에서 http redirect to https 세팅하기

---

cloudflare 같은 dns 서비스를 사용하지 않으면 https redirect를 위해 nginx에 직접 rewrite 코딩해야합니다. 저는 AWS에 docker로 API를 배포해서 AWS ECS 세팅을 추가로 설명하겠습니다.


## nginx server 세팅

```
server {
    listen                  80;
    server_name             <user server url>;
    rewrite                 ^ https://$server_name$request_uri? permanent;
}

server {
    listen                  1443;
    server_name             <user server url>;
    charset                 utf-8;
    client_max_body_size    128M;


    location / {
            uwsgi_pass      unix:///tmp/app.sock;
            include         uwsgi_params;
    }

}
```

## Elastic load balencer 세팅

EC2 인스턴스 앞에 ELB가 있으면 health check를 위해 아래 세팅을 해야합니다.

1. ELB 포트 구성: 80 -> 80 and 443 -> 1443
2. Health check 타겟은 TCP:1443으로 설정합니다. 80포트로 설정하면 301 redirect로 Health check에 실패합니다.



## Dockerfile 세팅
컨테이에 80 포트와 1443 포트를 열어줍니다.

```
EXPOSE      80  1443
```

## AWS ECS + EC2 세팅
마지막으로 도커의 Task definition 과 EC2 security group 설정을 해줍니다.

1. EC2 security group inbound 규칙에 80, 443, 포트 추가합니다.
2. 컨테이너 추가에 에 80:80, 1443:1443 포트를 매핑합니다.

---

### reference
- https://goo.gl/i7AioL
