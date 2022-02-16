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