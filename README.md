# Hyperledger_study

- 환경 Ubuntu18.04 LTS 
- 설치
- - Git
```
apt-get install git
```
- - curl
```
sudo apt-get install curl
```
- - docker
```
curl -fsSL https://get.docker.com/ | sudo s

sudo usermod -aG docker $USER //user 계정 추가

sudo reboot

docker -v
```
- - docker-compose
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```
- - Go
```
cd /usr/local  //go를 설치할 위치
sudo wget  https://golang.org/dl/go1.15.7.linux-amd64.tar.gz //파일 다운로드
sudo tar -xvf go1.15.7.linux-amd64.tar.gz //압출풀기


Go는 환경변수 설정을 해주어야 합니다. 

GOROOT,GOPATH,PATH를 설정해주어야하는데요. 

GOROOT 는 Go 패키지가 설치된 위치입니다. 

GOPATH 는 작업 디렉토리 위치입니다.  

PATH에 명령어의 경로를 입력하지 않으면 명령어를 찾지 못합니다. 

 

Go 패키지가 설치된 곳이 /usr/local/go 이고 작업 디렉토리 위치는 $HOME/workspace 로 잡았습니다. 

export GOROOT=/usr/local/go

export GOPATH= $HOME/workspace

echo $GOPATH

echo $GOROOT

export PATH=$PATH:$GOROOT/bin:$GOPATH/bin #PATH 에 GOPATH와 GOROOT 추가
echo "PATH=$PATH:$GOROOT/bin:$GOPATH/bin" >> ~/.bashrc
echo "PATH=$PATH:$GOROOT/bin:$GOPATH/bin" >> ~/.bash_profile
```
- - 하이퍼레저 패브릭 v2.2
```
curl -sSL https://bit.ly/2ysbOFE | bash -s -- <fabric_version> <fabric-ca_version> //여기서 버전 설정을 해주면 됩니다.

curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.2.1 1.4.9 #2.2 쓰실 분은 여기
fabric-sample 의 PATH 를 또 추가해주어야합니다. 

 

export PATH=/home/chung/workspace/fabric-samples/bin:$PATH
```


## 디렉터리 들어가기
```
cd fabric-samples/test-network
```

## network.sh의 옵션 및 명령어 파악하기
./network.sh 모드
```
up  -orderer와 피어 노드를 네트워크에 올림
up createChannel -채널을 네트워크에 올림
createChannel -네트워크가 생성된 후 채널을 생성하고 참여시킴
deployCC - 채널에 체인코드 구현
down - 네트워크를 닫음
```
아래는 up 과 createChannel 에서 사용
./network.sh 옵션 - network.sh up/network.sh createChannel
```
-ca<use CAs> :인증 기관을 통해네트워크 암호화 생성
-c<channel name>:만들 채널이름(기본값은 "mychannel")
-s<dbtype>:베포할 피어 상태의 데이터베이스(기본값goleveldb/couchdb)
-r<max>:특정시간동안 시도후 CLI시간초과(기본값5초)
-d<delay>:특정시간동안 CLI지연(기본값3초)
-i<image tag>:배포할 Fabric의 Docker 이미지 태그(기본값 latest)
-cai<ca_imagetag>:배포할 Fabric의 CA의 Docker이미지 태그(기본값latest)
-veredose:상세모드
```
deployCC 에서 사용할 수 있는 옵션입니다. 
./network.sh옵션 -deployCC
```
-c<channel name>:체인코드를 배포할 채널이름
-ccn<name>:체인 코드 이름
-ccl<language>:배포할 체인 코드의 프로그래밍 언어(기본값go)
-ccv<version>:체인코드 버전(기본값1.0)
-ccs<sequence>:체인코드 정의 시퀀스
-ccp<path>:체인코드 에 대한 파일 경로
-ccep<policy>:서명 정책 구문을 사용하는 체인 코드 보증 정책
-cccg<collection-config>:개인 데이터 collection구성 파일의 파일 경로
-cci<fcn name>:체인 코드 초기화 함수 이름,함수가 제공되면 init실행이 요청되고 함수가 호출
```
예시
```
network.sh up createChannel -ca -c mychannel -s couchdb -i 2.0.0
network.sh createChannel -c channelName
network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-javascript/-ccl javascript
```
모든 옵션 확인
```
 ./network.sh -h 
 ```
