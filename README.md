# Hyperledger_study
# 하이퍼레저 패브릭 공부
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
패브릭 네트워크는 오더링 서비스를 제공합니다. 하지만 피어가 트랜잭션의 순서나 새로운 블록으로 포함시키는 건 아닙니다. 부산 네트워크에서 피어들은 각자 돌아갑니다. 합의는 피어의 오버헤드를 이끌어낼 것입니다. 그럼 누가 정렬을 하나? 그건 ordering nodes, 즉 오더러(orderer)입니다. 오더러는 클라이언트로 부터 endorsed 트랜잭션을 받은 후 합의 알고리즘을 통해 트랜잭션의 순서를 정하고 그것을 블록에 추가합니다. 이 블록은 피어 노드들에게 전달되고 각 블록체인 원장에 블록을 추가 하게 됩니다. 

## 테스트네트워트 가져오기
test-network 디렉토리 내에서 ./network down 을 통해 혹시 네트워크가 열려있었거나 돌아가는 컨테이너들을 하는 것을을 닫아줍니다. 

- ./network up
![1번](https://user-images.githubusercontent.com/88940298/154184717-5f2c7f04-2e8c-498d-8a8c-9a8086cc14bb.png)


- test network 의 구성은 2개의 피어, 하나의 오더러, 하나의 클라이어트
- docker ps -a 명령어로 컨테이너를 확인 

![2번](https://user-images.githubusercontent.com/88940298/154184755-5ca6b24f-1078-46e5-b345-2faa95ba77dd.png)

- docker 명령어도 파악을 해두셔야 하는데 ps 는 실행하는 컨테이너 목록을 보여주고 -a 옵션은 종료되었던 컨테이너 목록까지 확인할 수 있습니다.

## 채널생성 
- 채널은 조직과 조직 간의 연결 입니다.제네시스 블록에 권한이 저장되어 있는데 이로 권한 판단을 합니다. 
- 저희는 두 개의 조직 org1, org2 를 가지고 있습니다. 
- 이제 network.sh 를 통해 채널을 org1, org2 사이에 채널을 만들고 그들의 피어를 채널에 join 시킬 겁니다. 
```
./network.sh createChannel 
```
- defalut 채널명은 myChannel 입니다. -c 옵션을 추어 변경 가능합니다. 
![3번](https://user-images.githubusercontent.com/88940298/154184807-56b15441-8de5-4ad4-97e4-65c1f7fab550.png)


- 채널2도 참여가 되고 각 조직에 앵커 피어들이 설치가 됩니다. 

![4번](https://user-images.githubusercontent.com/88940298/154184831-b67023a6-3e5b-4130-b419-d11298a64e01.png)


## 채널에서 체인코드 시작하기
- 하이퍼레저 패브릭에서 스마트 컨트랙트는 '체인코드'라고 불립니다. 이를 통해 채널의 원장과 상호작용할 수가 있습니다. 
```
./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
```
- 이름은 basic 위치는 ../asset-transfer-basic/chaincode-go 언어는 go 로 한다는 의미입니다.
![5번](https://user-images.githubusercontent.com/88940298/154184860-92f5eceb-f522-452a-b38b-71f3f9083831.png)


- 체인코드를 설치할 때 각 채널의 맴버들에 동의가 필요합니다. 필요한 수 만큼의 동의를 얻었을 때에 체인 코드의 정의가 채널에 커밋됩니다.

![6번](https://user-images.githubusercontent.com/88940298/154184897-d5491690-6434-416c-9775-47162f403b5d.png)

- 다음과 같이 합의를 걸칩니다. 
##  네트워크와 상호작용하기
- 피어의 CLI(Command Line Interface)를 통하여 네트워크와 상호작용할 수 있습니다
- CLI 를 통해 invoke(원장 업데이트 가능), 채널 업데이트/설치, 그리고 새로운 스마트 컨트랙트도 구현할 수 있습니다.
- 일단 PATH 추가를 더 해야합니다. 
- 그전에 yaml 파일에 대해 모르시는 분들도 있으실 거 같아 간단히 설명하자면, xml과 json 과 같이 데이터 포맷 형식입니다. yyyy-mm-dd 로 표현하냐 yyyy.mm.dd로 표현하냐를 정의해놓은 파일인데 yaml 은 xml과 json보다 가독성이 좋은 형식입니다. 아래 비교 사진을 통해 파악할 수 있습니다.

![7번](https://user-images.githubusercontent.com/88940298/154184949-a61ea2d1-ce8b-4f70-9660-c8c90fcf1d54.png)

- 그럼 tset-network 디렉토리에 있는 걸 확인하고 다음을 입력해줍니다.
```
export PATH=${PWD}/../bin:$PATH
아래도 설정해줍니다. 

export FABRIC_CFG_PATH=$PWD/../config/
 
```
- 이 후 아래 명령어들을 통해서 org1 에 peer CLI의 설정을 할 수 있습니다. 
```
 export CORE_PEER_TLS_ENABLED=true

export CORE_PEER_LOCALMSPID="Org1MSP"

export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt

export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp export CORE_PEER_ADDRESS=localhost:7051
```
![8번](https://user-images.githubusercontent.com/88940298/154184970-14f05fa0-29fc-48ce-bfb0-f3163de181d4.png)


- test-network 안에 organizations 폴더를 확인하시면 됩니다. 
- 이제 아래의 명령어를 통해서 원장을 초기화 할 수 있습니다. 
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"InitLedger","Args":[]}'
```
- 만약 성공하면 다음과 같이 출력이 됩니다. 
```
-> INFO 001 Chaincode invoke successful. result: status:200
```
- 이제 원장에 쿼리를 날릴 수도 있습니다. 모든 Assets을 가져오는 것이라고 직관적으로 이해할 수 있습니다. 
```
peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}'
```
- 만약 성공하면 다음과 같이 출력이 됩니다. 
```
[ {"ID": "asset1", "color": "blue", "size": 5, "owner": "Tomoko", "appraisedValue": 300},

{"ID": "asset2", "color": "red", "size": 5, "owner": "Brad", "appraisedValue": 400},
{"ID": "asset3", "color": "green", "size": 10, "owner": "Jin Soo", "appraisedValue": 500},
{"ID": "asset4", "color": "yellow", "size": 10, "owner": "Max", "appraisedValue": 600},

{"ID": "asset5", "color": "black", "size": 15, "owner": "Adriana", "appraisedValue": 700},

{"ID": "asset6", "color": "white", "size": 15, "owner": "Michel", "appraisedValue": 800} ]

 
```
- 에셋(자산) 들의 색깔, 사이즈,소유자, 평가된 가치가 출력이 됩니다. 

- Query 와 Invoke 의 차이는 원장의 변경 가능 여부 입니다. 간단히 Read/Write라고 생각할 수 있습니다. 
- 이제는 원장을 변경하는 Invoke 를 날려보겠습니다. 
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"TransferAsset","Args":["asset6","Christopher"]}'
```

- 이 명령어 후 다시 GetAllAsset을 치면 다음과 같이 asset6 의 소유자가 Christopher로 변한 것을 확인할 수 있습니다. 
- 조직 2에서 이 에셋을 확인을 해보겠습니다. 
- 조직 1과 동일하게 환경 설정을 해줍니다. 
```
# Environment variables for Org2

export CORE_PEER_TLS_ENABLED=true export CORE_PEER_LOCALMSPID="Org2MSP"

export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt

export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp

export CORE_PEER_ADDRESS=localhost:9051

 
```
- 이제 조직 2에 쿼리를 날릴 수 있습니다. 
```
peer chaincode query -C mychannel -n basic -c '{"Args":["ReadAsset","asset6"]}'
```
- 다음과 같은 동일한 결과를 확인할 수 있습니다.
```
{"ID":"asset6","color":"white","size":15,"owner":"Christopher","appraisedValue":800}
```

## 네트워크 닫기
이제 테스트 네트워크를 닫아보겠습니다. 

이 때 노드와 체인코드 컨테이너는 먹추고 조직읜 암호화 부분이 삭제되고 도커로 부터 체인코드의 이미지가 삭제 됩니다. 

도커나 채널들을 정리해줘서 다음 ./network.sh up 이 돌아갈 수 있는 환경을 만들어 줍니다. 
```
./network.sh down
```
![9번](https://user-images.githubusercontent.com/88940298/154185003-69ae186c-de2e-4917-955d-452aedd08c1b.png)


