# Day 1 - AWS EC2 & Nginx Setup

## 진행 사항

### 1. AWS 환경 준비

-   AWS 계정 생성 및 프리 티어 활성화
-   EC2 인스턴스 생성(t3.micro)
    -   OS: Amazon Linux 2023 kernel-6.1 AMI
    -   보안그룹: SSH(22), HTTP(80), HTTPS(443) 허용

### 2. 기본 패키지 업데이트

```bash
sudo yum update -y
```

### 3. 웹 서버 설치 및 확인

```bash
sudo yum install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

-   브라우저 접속: http://13.211.105.248/
-   Nginx 기본 화면 (“Welcome to Nginx”) 확인
    ![Nginx 기본 화면](/home/ec2-user/devops-lab-ec2/images/nginx_welcome.png)

### 4. Git 연결

-   GitHub Repository 생성
-   EC2에서 OpenSSH 키 생성 및 GitHub SSH Keys 등록

```bash
ssh-keygen -t ed25519 -C "injin0318@gmail.com"
cat ~/.ssh/id_ed25519.pub  # GitHub에 복사
```

-   Git 초기화 및 원격 연결

```bash
git init
git remote add origin git@github.com:INgenious-with/devops-lab-ec2.git
```
