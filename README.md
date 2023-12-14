# README

이 레포지토리는 아래 레포지토리에 대한 래퍼로 동적 웹서버를 구축하는 실습하기 위한 앱 실습 자료입니다.
- https://github.com/noeul1114/pragmatic

---

## 인스턴스 구성


#### 필요 패키지 설치
```bash
# 시스템 업데이트
sudo yum update -y

# 필요한 패키지 설치
sudo yum install -y amazon-efs-utils
sudo yum install -y git
sudo yum install -y docker
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Docker 서비스 활성화 (부팅 시 자동 시작)
sudo systemctl enable docker

# Docker 서비스 시작
sudo service docker start

```

#### EFS 마운트
```bash
EFS_DNS="fs-0540cd72fa90bdb74.efs.ap-northeast-2.amazonaws.com"
EFS_DNS_ID=$(echo $EFS_DNS | cut -d. -f1)
sudo mkdir -p /mnt/efs
sudo cp /etc/fstab /etc/fstab.bak
echo "$EFS_DNS_ID:/ /mnt/efs efs _netdev,tls 0 0" | sudo tee -a /etc/fstab
sudo mount -a
sudo mkdir /mnt/efs/media
sudo mkdir /mnt/efs/static
```

#### RDS 연결 및 DB 생성
- db 클라이언트 설치
```bash
# db 클라이언트 설치
sudo dnf update -y
sudo dnf install mariadb105-server
```
- DB 접속
```bash
# mysql 접속
mysql -u admin -p -h database-1.clh71cfeiyna.ap-northeast-2.rds.amazonaws.com
```
- 테이블 생성
```bash
CREATE DATABASE my_new_database;
SHOW DATABASES;
```


#### ssh 키 생성
- root 사용자 권한 변경
```bash
sudo su -
```

- 키 생성
```bash
# SSH 디렉토리로 이동
cd ~/.ssh/
# 비밀번호 없는 SSH 키 생성
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ""
# ssh-agent 실행
eval $(ssh-agent -s)
# 생성한 키를 ssh-agent에 추가
ssh-add ~/.ssh/id_ed25519
```

- 퍼블릭 키 확인
```bash
# 퍼블릭 키 확인 및 복사
cat id_ed25519.pub
```

- 연결 확인
```bash
mkdir ~/test-clone
cd ~/test-clone
git clone git@github.com:Dolphin98/webserver-lecture.git
```


## 시작 템플릿
```bash
#!/bin/bash
git clone git@github.com:Dolphin98/webserver-lecture.git
cd webserver-lecture
id=$(hostname -I | cut -d' ' -f1)
sed -i "s/<title>.*<\/title>/<title>$id<\/title>/" app/templates/head.html
docker-compose up -d
```