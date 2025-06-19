# Drosera Network 한국 가이드

**Drosera 테스트넷 기여**
- Trap deploy 해보기
- 오퍼레이터 노드 실행해보기

  
# 최소사양 & 준비물 #
- 2 CPU Cores
- 4 GB RAM
- 20 GB Disk Space
- VPS 
- Ethereum Holesky RPC 준비 [Alchemy](https://www.alchemy.com/) or [Ankr](https://www.ankr.com/rpc/)


#### 시스템 패키지 목록 업데이트 및 업그레이드
```
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install curl ufw iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y
```

#### 도커 설치
```
sudo apt update -y && sudo apt upgrade -y

sudo apt-get remove -y docker.io docker-doc docker-compose podman-docker containerd runc

sudo apt-get install -y ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y

sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo docker run hello-world
```

# TRAP deploy

## 1.필요 환경 구성

#### Drosera CLI 설치
```
curl -L https://app.drosera.io/install | bash
```
```
source ~/.bashrc
```
```
droseraup
```


#### Foundry 설치
```
curl -L https://foundry.paradigm.xyz | bash
```
```
source ~/.bashrc
```
```
foundryup
```


#### Bun 설치
```
curl -fsSL https://bun.sh/install | bash
```
```
source ~/.bashrc
```


## 2. Trap 실행 : creating & deploy
```
mkdir my-drosera-trap
```
```
cd my-drosera-trap
```

### "Github_Email" , "Github_Username" 본인 이메일이랑 닉네임으로 바꾸기
```
git config --global user.email "Github_Email"
git config --global user.name "Github_Username"
```

### Trap 실행하기
```
forge init -t drosera-network/trap-foundry-template
```

### Trap Compile 하기
```
curl -fsSL https://bun.sh/install | bash

source /root/.bashrc

bun install
```
```
forge build
```

![image](https://github.com/user-attachments/assets/35024926-1880-4332-a083-be2356fc71b1)


### Trap deploy 하기

- < > 부분 EVM 지갑 프빗키키로 바꾸기 / Holesky ETH faucet 해놓기 [링크](https://cloud.google.com/application/web3/faucet/ethereum/holesky)
- 뭐뜨면 ofc 입력후 Enter
```
DROSERA_PRIVATE_KEY= < > drosera apply
```

![image](https://github.com/user-attachments/assets/7f07bd7b-e8df-4696-93d7-24d3664f73d5)
빨간색 네모부분 본인 Trap 주소


## 3. Dashboard에서 Trap 확인하기

- https://app.drosera.io/ 접속후 지갑연결
- Traps Owned 클릭
  
![image](https://github.com/user-attachments/assets/5937266d-6a89-459f-a2d0-3111de3efa7f)


## 4. Trap Bloom boost 하기
- Send Bloom Boost 클릭후 Holesky ETH deposit 하기


![image](https://github.com/user-attachments/assets/78baebaa-8f71-4511-803c-10e35f44ccb4)


## 5. Block fetch
```
drosera dryrun
```

---------------------------------------------------------------------------------------

# Operater 노드 


## 1. 주소 화이트리스트 시키기
```
cd my-drosera-trap
nano drosera.toml
```
- 코드 실행후 나오는 창에서 아래 부분이 보일텐데 [] 안에 "본인 EVM 주소" 넣기
```
whitelist = []  => whitelist = ["0x1234567"]
```
아래 코드 추가하기
```
private_trap = true
```
- crtl + o , Enter 로 저장후 crtl + x 나오기

- < > 부분 EVM 지갑 프빗키키로 바꾸기
```
DROSERA_PRIVATE_KEY= < > drosera apply
```

![image](https://github.com/user-attachments/assets/3761f45e-f824-454f-a45f-7952d2989b9c)
- 화이트리스트가 완료되면 Trap이 Private가 된걸 볼수있음


## 2. Operater CLI 설치
```
cd ~
```
```
curl -LO https://github.com/drosera-network/releases/releases/download/v1.19.0/drosera-operator-v1.19.0-x86_64-unknown-linux-gnu.tar.gz
```
```
tar -xvf drosera-operator-v1.19.0-x86_64-unknown-linux-gnu.tar.gz
```
```
drosera-operator
```
![image](https://github.com/user-attachments/assets/5669958c-f211-4158-91fe-03ef3d8932cf)


## 3. Docker image 설치
```
docker pull ghcr.io/drosera-network/drosera-operator:latest
```

## 4. Operater 등록하기
```
drosera-operator register --eth-rpc-url https://ethereum-holesky-rpc.publicnode.com --eth-private-key PV_KEY
```
- 코드 맨끝에 PV_KEY 부분 EVM 지갑 프빗키로 바꾸기
![image](https://github.com/user-attachments/assets/b18bbcb5-6d59-40fd-8e8e-ec49622cef94)

## 5. 포트 열기
```
sudo ufw allow ssh
sudo ufw allow 22
sudo ufw enable

sudo ufw allow 31313/tcp
sudo ufw allow 31314/tcp
```
## 6. Operater 실행하기

```
sudo tee /etc/systemd/system/drosera-operator.service > /dev/null <<EOF
 
[Unit]
Description=Service for Drosera Operator
Requires=network.target
After=network.target
 
[Service]
Type=simple
Restart=always
 
Environment="DRO__DB_FILE_PATH=/var/lib/drosera-data/drosera.db"
Environment="DRO__DROSERA_ADDRESS=0xea08f7d533C2b9A62F40D5326214f39a8E3A32F8"
Environment="DRO__LISTEN_ADDRESS=0.0.0.0"
Environment="DRO__ETH__CHAIN_ID=17000"
Environment="DRO__ETH__RPC_URL= <<https://ethereum-holesky-rpc.publicnode.com>>"
Environment="DRO__ETH__BACKUP_RPC_URL=https://1rpc.io/holesky"
Environment="DRO__ETH__PRIVATE_KEY=<<YOUR_ETH_PRIVATE_KEY_HERE>>"
Environment="DRO__NETWORK__P2P_PORT=31313"
Environment="DRO__NETWORK__EXTERNAL_P2P_ADDRESS=<<YOUR-PUBLIC-VPS-IP-ADDRESS>>"
Environment="DRO__SERVER__PORT=31314"
 
ExecStart=/home/root/.drosera/bin/drosera-operator node
 
[Install]
WantedBy=multi-user.target
EOF
```
- << >> 한 부분 수정해서 넣기 / 변경할부분 RPC 주소 , EVM 프빗키, VPS ip
- 혹시나 ExecStart 부분에 사용자면 root 아닌 사람들 변경 

```
sudo systemctl daemon-reload
```
- 시스템 파일불러오기
```
sudo systemctl start drosera-operator.service
```
- Operater 실행하기
```
sudo systemctl enable drosera-operator.service
```
```
sudo systemctl status drosera-operator.service
```
- Operater 상태 확인하기. 아래 그림처럼 나오면 성공!!!!!!!!!!!!!!!!!!!!!!
![image](https://github.com/user-attachments/assets/5368f72a-c5bb-46a6-b7b3-eafbbe4dc19f)

![image](https://github.com/user-attachments/assets/f3a6453d-ea83-4315-b7fc-0521f57702eb)

## 7. Trap Opt-in 하기

![image](https://github.com/user-attachments/assets/d2389b8f-bde3-422d-866c-fe947ab7ff40)

- Opt in 클릭 후 진행
- Operater와 Tarp 연결과정

## 8. Operater 노드 상태 체크
![image](https://github.com/user-attachments/assets/637ac0e3-705d-4dbf-98c2-79cc973a5a71)








