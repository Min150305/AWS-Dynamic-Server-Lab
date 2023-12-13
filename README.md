# README

이 레포지토리는 아래 레포지토리에 대한 래퍼로 동적 웹서버를 구축하는 실습하기 위한 앱 실습 자료입니다.
- https://github.com/noeul1114/pragmatic

---

## 1. 보안그룹 생성
- EC2 보안그룹 : HTTP, HTTPS, SSH
- EFS 보안그룹 : NFS
- DB 보안그룹 : 3306
- 로드벨런서 : HTTPS

## 2. EFS 생성

## 3. DB 생성



nfs 보안그룹 설정 후

### 필요 패키지 설치
```bash
# 시스템 업데이트
sudo yum update -y

# 필요한 패키지 설치
sudo yum install -y amazon-efs-utils
sudo yum install -y git
sudo yum install -y docker
sudo yum install -y python3-pip
sudo pip3 install --ignore-installed requests
sudo pip3 install docker-compose

# Docker 서비스 활성화 (부팅 시 자동 시작)
sudo systemctl enable docker

# Docker 서비스 시작
sudo service docker start
```

### EFS 마운트
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

###




