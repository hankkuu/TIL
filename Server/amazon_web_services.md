# AWS - Amazon Web Services

단순히 웹 호스팅, 서버라고 지칭할 수 없을만큼 거대한 서비스다. 클라우드 서비스이며 "서버의 구매, 구축, 운영을 대행"해준다. 가상화 기술을 통해 물리적 서버를 잘게 쪼개 필요한 사람에게 필요한 만큼만 제공해줄 수 있고, 오토 스케일링, 종량제(사용한 만큼만 과금) 등 다양한 기능이 존재한다. 개발자에게 필수 서비스이다.

## 1. 대표적 서비스들

### 1.1 EC2 - Elastic Compute Cloud

- 독립적인 컴퓨터 한 대를 아마존 인프라에 설치한다고 생각하면 됨
- 웹서버, 애플리케이션 서버로 사용.
- T가 일반적인 것이고 M은 메모리에서 더 좋은 것, C는 CPU 성능이 더 좋은 것이다. 앞에 Family 칸에 붙어있는 설명을 보면 대충 필요한 것을 알 수 있다.
- EBS: EC2의 저장용량을 의미한다. 30기가까지는 무료다. 리눅스는 8기가로 오케이. 그리고 Volume Type은 저장장치의 종류인데 마그네틱이 가장 느리고 provisioned는 IOPS 수치를 높게 해서 성능을 더 높일 수 있다. "Delete on Terminations"을 체크하면 EC2를 terminate했을 때 EBS도 같이 지워진다. 체크를 안하면 남아있다.
- Tag는 Key, Value 쌍으로 여러가지 입력해줄 수 있다.
- Security Group: 네트워크에서 어떤 타입으로(SSH, HTTP, HTTPS 등) 인스턴스에 접근하는 것을 허용할 것인가에 대한 것이다. 기본적으로 터미널로 접근할 때는 SSH가 Anywhere로 돼있으면 된다. 만약 SSH가 My IP로 돼있으면 현재 위치 IP가 자동으로 적용된다. 또한 해당 인스턴스를 웹서버로 이용한다면 HTTP 타입으로 열어서 Anywhere로 해주면 된다. 만약 윈도우 서버라면 RDP 타입이 추가로 필요하다.
- 비밀번호: 매우 긴 길이의 문자로 이루어져있는 pem 파일을 이용한다. 새로 만들거나, 기존에 있는 것을 이용할 수 있다. 한 번 파일 받으면 다시는 제공받지 못하기 때문에 잘 간수해야함.
- 연결하기: 인스턴스 우클릭해서 connect 눌러보면 설명 나온다. `ssh -i "webs_server.pem" ubuntu@54.249.221.234`로 들어가고 `exit`으로 빠져나온다.

### 1.2 S3 - Simple Storage Service

- 파일 서버. 무제한으로 저장 가능하기 때문에 파일 서버로 사용한다. EC2는 용량 제한이 있다.
- 스케일은 아마존 인프라가 담당하기 때문에 동접 트래픽 걱정 안해도 된다.
- 단일 파일이 1 byte에서 5 tera byte 까지 가능하다.

### 1.3 RDS - Relational Database Service

- MySQL, Oracle, SQL Server 지원
- 백업, 리플리케이션같은 것을 아마존 인프라가 제공하기 때문에 굉장히 편리

### 1.4 ELB - Elastic Load Balancing

- EC2로 유입되는 트래픽을 여러대의 EC2로 부하 분산
- 인스턴스를 항상 체크해서 장애가 발생한 EC2는 자동으로 배제.
- Auto Scaling 기능 존재. 트래픽에 따라서 자동으로 똑같은 이미지 EC2를 생성하고 삭제한다.(Cloud watch)

## 2. Region, Availability zone

- Region: 데이터 센터. 서비스 할 지역과 가까워야 한다. 내 위치에서 하는게 아니다. [cloudping](http://www.cloudping.info)에서 현재 접속 지역과 각 리전에 대한 속도를 확인할 수 있다.
- AZ: 한 region에서 백업 등의 이유로 다른 zone이 존재한다. 서울은 2개, 일본은 3개 등이 있다. 자연재해나 장애로 인해 문제가 발생할 소지가 있기 때문에 존재한다. 같은 region에서 각 AZ들은 전용선으로 연결돼있어서 매우 빠른 속도로 데이터 백업 등이 가능하다.

## 3. 인스턴스 종류

- On-demand: 사용하는 만큼만 내는 가장 일반적인 과금 정책
- 예약: 미리 결제하면 할인 받는 것. 1년, 3년 미리 결제 가능하다.
- 스팟: AWS의 노는 컴퓨터가 많아지면 가격이 실시간으로 떨어진다. bidding이 들어감.

## 4. Marketplace

- 다른 사람들이 만든 이미지를 쓸 수 있다. AWS Marketplace에서 보면 사람들이 올린 이미지를 볼 수 있다. 더 쉽게는 https://aws.amazon.com/marketplace 로 들어가서 원하는것을 고르면 된다. 대표적으로 wordpress(HVM 버전 선택하는게 좋다.)
- 이미지에 대한 가격과 AWS 사용 가격이 따로 표시돼있으니 총 합계 가격을 잘 확인해야 한다.

## 5. 아키텍쳐

![Imgur](http://i.imgur.com/Xwr0JeH.png)

- 위와 같은 구조가 기본적으로 자주 쓰인다고 한다. AWS Beanstalk을 활용하면 쉽게 한 번에 구축 가능하다.
- 관련 링크: [pdf](https://github.com/cloudtrack/yonsei-class)