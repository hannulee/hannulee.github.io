---
title: 도커 기초
category: docker
order: 1
---

docker exe [container] [command]  

*#bash shell로 접속* 
docker exec -i -t my-nginx bash

*#환경변수 확인*
docker edec my-nginx env  

+ docker rm -f $(docker ps -a -q);docker container prune
+ docker run -d --name my-nginx nginx
+ docker exec -i -t my-nginx bash
+ docker ps

veth: virtual eth
docker0: 도커 엔진에 의해 기본 생성되는 브릿지 네트워크

Continer #1, eth0 --- veth ---  
　　　　　　　　　　　　　　--- docker0 --- eth0  
Continer #2, eth0 --- veth ---

docker run -p [HOSTIP:PORT]:[CONTAINER PORT] [container]
#nginx 컨테이너의 80번 포트를 호스트 모든 IP의 80번 포트와 연결하여 실행
+ docker run -d -p 80:80 nginx
호스트의 127.0.0.1 아이피의 80포트와 컨테이너의 80포트를 연결하여 실행
+ docker -run -d 127.0.0.1:80:80 nginx
#nginx 컨테이너의 80번 포트를 호스트의 포트와 랜덤하게 연결하여 실행
+ docker run -d -p 80 nginx

+ curl localhost:80
+ docker rm -f [CONTAINER_ID]

+ docker run -d --expose 80 --name nginx-expose nginx #expose 옵션은 문서화용도

+ docker network ls
+ Native Drivers = Bridge, Host, None, Overlay
+ Remote Drivers = 3rd-Party Plugins

+ Single Host Netwirking --- Driver --- bridge, host, none
+ Multi Host Networking --- Driver --- overlay

none

+ docker run -i -t --net none ubuntu:focal
+ docker inspect [CONTAINER_ID]
+ apt update

host
+ docker run -d --network=host grafana/grafana

bridge
+ docker-network create --driver=bridge hnlbridge
+ docker run -d --network=hnlbridge --net-alias=hello nginx
+ docker run -d --network=hnlbridge --net-alias=grafana grafana/grafana
+ docker exec -it [CONTAINER_ID] bash
+ wget hello
+ cat index.html
+ curl grafana:3000
+ ifconfig 로 확인해보면 컨테이너 갯수만큼 가상의 veth가 생성된 것을 확인 할 수 있다.

### 도커 레이어 아키텍처
1. docker run app, Read Write, Container Layer
    a. Layer 6 : Container Layer
2. docker build -t app ., Read Only, Image Layers
    a. Layer 5 : Update Entrypoint
    b. Layer 4 : Source code
    c. Layer 3 : Install in pip packages
    d. Layer 2 : Changes in apt packages
    e. Layer 1 : Base Ubuntu Layer


host volumne
+ docker run -d --name nginx -v /opt/html:/usr/share/nginx/html nginx
※ host의 /opt/html디렉토리룰 nginx의 웹 루트 디렉트리롤 마운트



(Kubernetes와 Docker로 한 번에 끝내는 컨테이너 기반 MSA, 패스트캠퍼스, 20230810)

