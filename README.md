 





Study  

## 목차  

[1. GIT / GITHUB](#1-git--github)  
[2. Docker](#2-docker)  

> [1. Container 란](#1-container-란)  
> [2. Image 란](#2-image-란)  
> [3. Layer 란](#3-layer-란)  

 마크다운 참조 링크 : [링크1](https://gist.github.com/ihoneymon/652be052a0727ad59601#23-%EB%AA%A9%EB%A1%9D) / [링크2](https://heropy.blog/2017/09/30/markdown/) / [링크3](https://wikidocs.net/1678#_1)  
 하루패드 : [하루패드 다운로드 링크](http://pad.haroopress.com/user.html)  

------

## 1. GIT / GITHUB

- GIT 은 일반적인 SVN과 같은 소스 버전 관리 시스템을 말한다. 또한 분산 버전 관리 시스템이다.  
- GITHUB는 GIT 레파지토리를 인터넷 상에 제공하는 서비스라고 볼수 있다.  
- GIT 참고링크 : [GIT 링크](https://www.slideshare.net/einsub/svn-git-17386752)  
- GIT 구조

> <img width="500" src="https://image.slidesharecdn.com/svngit-130319211210-phpapp02/95/svn-git-24-1024.jpg?cb=1374635099">  
>
> - [GIT 공홈 메뉴얼](https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%EB%B2%84%EC%A0%84-%EA%B4%80%EB%A6%AC%EB%9E%80%3F)  

## 2. Docker

- Docker는 컨테이너 기반의 오픈소스 가상화 플랫폼  
  - [Docker 참고 URL](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)  
  - [Docker 참고 URL2](https://seunguklee.github.io/2018/02/13/what-is-docker/#%EB%8F%84%EC%BB%A4-Docker-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

### 1. Container 란

> - 격리된 공간에서 프로세스가 동작하는 기술

### 2. Image 란

> - 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는것

### 3. Layer 란

> - Container와 Image를 효율적으로 관리하기 위해 사용하는 방식
>   <img width="500" src="https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/image-layer.png">  

### 4. Docker 환경 설정

[참고링크](https://docs.docker.com/)



#### Docker 설치

Docker 공홈에서 파일 다운로드 후 설치 [링크](https://store.docker.com/editions/community/docker-ce-desktop-mac?tab=description)

설치 완료 후 Docker 실행 할 때, 꼭 계정(이메일이 아닌 ID)을 저장하여 Docker를 실행한다.

docker 명령어 테스트 할 경우, docker hub에 있는 소스 파일들을 가져와야 하기 때문이다.

아래는 Docker가 정상적으로 설치 되었나 확인하는 명령어 목록이다.

```bash
## List Docker CLI commands
docker
docker container --help
## Display Docker version and info
docker --version
docker version
docker info
## Execute Docker image
docker run hello-world
## List Docker images
docker image ls
## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls --all
docker container ls -aq
```



#### Dockerfile 정의

컨테이너 내부 환경을 정의함

런타임 실행 환경에 대한 이미지 파일 생성에 필요한 정보가 들어가 있음

```bash
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```



#### 실제 구동할 APP 런타임 소스 작성

requirements.txt

```txt
Flask
Redis
```

app.py

```python
  from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
  try:
      visits = redis.incr("counter")
  except RedisError:
      visits = "<i>cannot connect to Redis, counter disabled</i>"

  html = "<h3>Hello {name}!</h3>" \
         "<b>Hostname:</b> {hostname}<br/>" \
         "<b>Visits:</b> {visits}"
  return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
  app.run(host='0.0.0.0', port=80)

```



#### Image 생성

docker build -t 명령어를 이용하여 Image를 생성

```bash
docker build -t friendlyhello .
```



#### APP 런타임 소스를 실행

docker run -p 명령어를 이용하여 APP을 실행

```bash
docker run -p 4000:80 friendlyhello
```



#### 정상적으로 APP 이 실행되는지 확인

localhost:4000 으로 브라우저에서 확인함(실제 0.0.0.0/80 으로 구동이 되었지만 docker에서 run을 할때, 4000 포트로 매핑을 해주었기 때문에 4000 포트로 확인)

아래 명령어로 실행되고 있는 container 가 있는지 확인

```bash
  docker container ls
```



#### 실행 중인 컨테이너 중단

아래 명렁어를 통하여 실행중인 컨테이너를 중지 시킴

```bash
docker container stop [CONTANINER ID]
```



#### [hub.docker.com](https://hub.docker.com/)에 로그인

다음 명령어를 통해 hub.docker.com의 계정에 연동

```bash
$ docker login 
```



#### Image에 Tag 하기

로컬에 있는 Image를 [hub.docker.com](https://hub.docker.com/) 레지스트리에 연결하는 표기법은 `username/repository:tag` 입니다.

Image에 Tag를 추가하는 docker 명령어

```bash
docker tag image username/repository:tag 
```

For example :

```bash
docker tag friendlyhello gordon/get-started:part2
```

Image에 Tag가 되었는지 확인

```bash
$ docker image ls
```



#### Image 배포 하기

위에 생성한 태그된 이미지를 배포

```bash
docker push username/repository:tag
```

docker hub에 로그인이 되어있는 상태라면, docker hub 레파지토리에서 확인 가능



#### Remote repository 에 있는 Image 로 실행하기

`docker run` 명령어를 이용하여 APP을 실행

```bash
docker run -p 4000:80 username/repository:tag
```

만약 로컬 환경에 Image 파일이 없다면, Docker는 remote repository에서 Image 를 pull 해 옵니다.

```bash
$ docker run -p 4000:80 gordon/get-started:part2
Unable to find image 'gordon/get-started:part2' locally
part2: Pulling from gordon/get-started
10a267c67f42: Already exists
f68a39a6a5e4: Already exists
9beaffc0cf19: Already exists
3c1fe835fb6b: Already exists
4c9f1fa8fcb8: Already exists
ee7d8f576a14: Already exists
fbccdcced46e: Already exists
Digest: sha256:0601c866aab2adcc6498200efd0f754037e909e5fd42069adeff72d1e2439068
Status: Downloaded newer image for gordon/get-started:part2
 * Running on http://0.0.0.0:80/ (Press CTRL+C to quit)
```

------



#### `docker-compose.yml` file

docker에서 확장이 필요한 서비스가 있을때, 서비스를 확장하려면 해당 소프트웨어를 실행하는 컨테이너 인스턴스 수를 변경하여 서비스에 더 많은 컴퓨팅 리소스를 할당하게 합니다. 이럴 경우, Docker 플랫폼으로 서비스를 정의, 실행 및 확장하는 것은 매우 쉽습니다. `docker-compose.yml` 파일을 작성하기 만하면됩니다.

`docker-compose.yml` 파일은 Docker 컨테이너가 운영 환경에서 어떻게 작동해야하는지 정의하는 YAML 파일입니다.

```yaml
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
      restart_policy:
        condition: on-failure
    ports:
      - "4000:80"
    networks:
      - webnet
networks:
  webnet:
```

위의 `docker-compose.yml` 파일은 Docker에게 다음을 수행하도록 지시합니다.

- 업로드 한 이미지를 레지스트리에서 가져옵니다.
- 이미지의 5 개의 인스턴스를 `web`이라는 서비스로 실행하고 각 이미지를 최대 10 %의 CPU (모든 코어에서 사용)와 50MB의 RAM으로 제한합니다.
- 컨테이너 구동이 실패하면 restart 합니다.
- 호스트의 포트 4000을 `web`이라는 서비스의 포트 80에 매핑합니다.
- `webnet`이라는 부하 분산 네트워크를 통해 포트 80을 공유하도록 웹의 컨테이너에 설정합니다. (내부적으로 컨테이너는 자체적으로 임시 포트에서 웹 포트 80에 게시됩니다.)
- 기본 설정으로  `webnet` 네트워크를 정의합니다.



#### 새로운 load-balanced app 을 실행하기

`docker stack deploy` 명령어를 사용하기 전, 아래 명령어를 먼저 실행합니다.

> Note : `docker swarm init` 를 실행하지 않으면 “this node is not a swarm manager.” 라는 오류가 발생합니다. 

이제 아래 명령어를 통하여, app name을 부여하여 실행합니다.

```bash
docker stack deploy -c docker-compose.yml getstartedlab
```

단일 서비스 스택은 배포 된 Image 의 컨테이너 인스턴스 5 개를 하나의 호스트에서 실행합니다.

```bash
docker service ls
```

위의 명령어를 통하여 service가 정상적으로 실행이 되는지 확인합니다. (본제본의 갯수, APP 이름,이미지 경로)

서비스에서 실행되는 단일 컨테이너를 Task 라고 합니다. Task 에는 `docker-compose.yml` 에서 정의한 `replicas`  수 만큼 unique ID가 제공됩니다. 서비스의 Task 리스트를 아래 명령어로 확인합니다.

```bash
docker service ps getstartedlab_web
```

또한, 특정 서비스를 제한하지 않고, host 시스템에 있는 모든 컨테이너를 확인하려면 아래 명령어를 이용하여 확인할 수 있습니다.

```bash
docker container ls -q
```

`curl -4 http : // localhost : 4000` 을 연속으로 여러 번 실행하거나 브라우저에서 해당 URL로 이동하여 몇 번 새로 고침하십시오.

<img width="500" src="https://docs.docker.com/get-started/images/app80-in-browser.png">

어느 쪽이든, 컨테이너 ID가 변경되면서 로드 밸런싱이 실행됩니다. 각 요청마다 5 개의 Task 중 하나가 라운드 로빈 방식으로 응답하도록 선택됩니다. 



#### Scale the app

`docker-compose.yml` 의 `replicas` 를 변경하고 `docker stack deploy ` 명령을 실행하여 응용 프로그램을 확장 할 수 있습니다.

```bash
docker stack deploy -c docker-compose.yml getstartedlab
```

docker는 적절하게 변경된 내용을 적용합니다. 기존에 실행된 stack을 다운시키거나, container 를 제거하지 않고 확장할 수 있습니다.

이제 `docker container ls -q` 를 실행하여 배포 된 인스턴스가 재구성 된 것을 확인합니다. replicas 을 확장하면 더 많은 Task 가 생겨서 더 많은 container 가 시작됩니다.



#### Take down the app and the swarm

- app 실행 중지

  ```bash
  docker stack rm getstartedlab
  ```

- Swarm 을 중지

  ```bash
  docker swarm leave --force
  ```

------



### Get Started, Part 4: Swarms

------



#### Understanding Swarm clusters







------

## 3. 쓸데없는 개발 참조

- MAC 터미널 글꼴 색상 적용 : [링크](http://minus-build.tistory.com/23)
- Oracle VirtualBox 설치 : [링크](
