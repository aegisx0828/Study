# Study  
## 목차  
[1. GIT / GITHUB](#1-git--github)  
[2. Docker](#2-docker)  
> [1. Container 란](#1-container-란)  
> [2. Image 란](#2-image-란)  
> [3. Layer 란](#3-layer-란)  


 마크다운 참조 링크 : [링크1](https://gist.github.com/ihoneymon/652be052a0727ad59601#23-%EB%AA%A9%EB%A1%9D) / [링크2](https://heropy.blog/2017/09/30/markdown/) / [링크3](https://wikidocs.net/1678#_1)  
 하루패드 : [하루패드 다운로드 링크](http://pad.haroopress.com/user.html)  

---

## 1. GIT / GITHUB
* GIT 은 일반적인 SVN과 같은 소스 버전 관리 시스템을 말한다. 또한 분산 버전 관리 시스템이다.  
* GITHUB는 GIT 레파지토리를 인터넷 상에 제공하는 서비스라고 볼수 있다.  
* GIT 참고링크 : [GIT 링크](https://www.slideshare.net/einsub/svn-git-17386752)  
* GIT 구조
> <img width="500" src="https://image.slidesharecdn.com/svngit-130319211210-phpapp02/95/svn-git-24-1024.jpg?cb=1374635099">  
* [GIT 공홈 메뉴얼](https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%EB%B2%84%EC%A0%84-%EA%B4%80%EB%A6%AC%EB%9E%80%3F)  

## 2. Docker
* Docker는 컨테이너 기반의 오픈소스 가상화 플랫폼  
	- [Docker 참고 URL](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)  
	- [Docker 참고 URL2](https://seunguklee.github.io/2018/02/13/what-is-docker/#%EB%8F%84%EC%BB%A4-Docker-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80)

### 1. Container 란
> - 격리된 공간에서 프로세스가 동작하는 기술

### 2. Image 란
> - 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는것

### 3. Layer 란
> - Container와 Image를 효율적으로 관리하기 위해 사용하는 방식
> <img width="500" src="https://subicura.com/assets/article_images/2017-01-19-docker-guide-for-beginners-1/image-layer.png">  

### 4. Docker 환경 설정
[참고링크](https://docs.docker.com/)
1. Docker 설치  
* Docker 공홈에서 파일 다운로드 후 설치 [링크](https://store.docker.com/editions/community/docker-ce-desktop-mac?tab=description)  
* 설치 완료 후 Docker 실행 할 때, 꼭 계정(이메일이 아닌 ID)을 저장하여 Docker를 실행한다.
	* docker 명령어 테스트 할 경우, docker hub에 있는 소스 파일들을 가져와야 하기 때문이다.
* Docker가 정상적으로 설치 되었나, 확인하는 명령어 목록
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






---
## 3. 쓸데없는 개발 참조
* MAC 터미널 글꼴 색상 적용 : [링크](http://minus-build.tistory.com/23)
