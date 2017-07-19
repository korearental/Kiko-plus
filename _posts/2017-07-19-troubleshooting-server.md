---
layout: post
title: "서버 장애 대응 매뉴얼"
description: "서버 장애 대응 매뉴얼"
date: 2017-07-19
tags: [troubleshooting, guide, dess]
comments: true
share: true
---


## 1. 서비스 상태 확인

### 1) 서버 상태 확인
- 장애 발생시 서버의 서비스 상태들을 확인합니다. (사진1)
- 서버 상태 버튼을 클릭하여 서버 별 상세 상태도 확인합니다. (사진2)
- 이상이 발생한 서비스는 붉은색으로 상태창에 표기됩니다. (사진3)
 
![사진1, 서버 상태](/images/troubleshooting_server/image1.png)
![사진2, 서버 별 상세 상태](/images/troubleshooting_server/image2.png)
![사진3, 비정상 서비스](/images/troubleshooting_server/image3.png)

### 2) 서비스 재시작
- 비정상 작동중인 서비스가 있으면 해당 서비스를 재시작 합니다.
- 서비스 재시작을 위해 먼저 서버로 들어가 로그인 한 뒤 터미널프로그램을 실행합니다 (사진4)
- $sudo su 커맨드를 입력하고 비밀번호를 입력하여 super user로 로그인합니다. (사진5)
  * Iscsi 서비스 재시작 시에 켜져 있는 클라이언트들은 블루스크린이 발생할 수도 있습니다.
- 문제가 있는 서비스를 각 커맨드를 사용하여 재시작 합니다.
- 데몬 서비스 재시작: service diskless-daemon restart (사진6)
- DHCP 서비스 재시작: service isc-dhcp-server restart (사진7)
- ISCSI 서비스 재시작: service iscsitarget restart (사진8)
- TFTP 서비스 재시작: service tftpd-hpa restart (사진9)


![사진4, 터미널 프로그램 실행](/images/troubleshooting_server/image4.png)
![사진5, 수퍼 유저 로그인](/images/troubleshooting_server/image5.png)
![사진6, 데몬 서비스 재시작](/images/troubleshooting_server/image6.png)
![사진7, DHCP 서비스 재시작](/images/troubleshooting_server/image7.png)
![사진8, 데몬 서비스 재시작](/images/troubleshooting_server/image8.png)
![사진9, DHCP 서비스 재시작](/images/troubleshooting_server/image9.png)

### 3) 관리자 웹페이지에 접근이 안되는 경우

![사진10, 웹서버 서비스 재시작](/images/troubleshooting_server/image10.png)

- 관리자 웹페이지에 접근이 안 될 경우에 웹서버 서비스를 재시작 합니다.
- 웹서버 서비스 재시작: service local-web-daemon restart (사진10)

## 2. 장애 유형

### 1) 클라이언트 부팅 장애
#### 1-1) Disk read error
- 이 문제는 트래픽의 과도한 집중으로 인해 발생합니다.
- 클라이언트 중 토렌트나 스팀 다운로드 등 과도한 트래픽을 유발할 수 있는 프로그램을 사용하는 유저가 있는지 확인합니다.
![사진11, disk read error](/images/troubleshooting_server/image11.png)

#### 1-2) no boot device
- 이 문제는 디스크가 제대로 생성되지 않은 경우에 발생합니다.
- 클라이언트 초기화 기능을 사용하여 해당 클라이언트의 디스크를 재생성해서 해결합니다. (사진13)

![사진12, no boot device](/images/troubleshooting_server/image12.png)
![사진13, 클라이언트 초기화](/images/troubleshooting_server/image13.png)

#### 1-3) check cable
- 이 문제는 클라이언트의 랜선이 제대로 연결되어 있지 않은 경우에 발생합니다.
- 해당 클라이언트의 랜선 및 NIC에 이상이 없는지 확인합니다.

![사진14, check cable](/images/troubleshooting_server/image14.png)

#### 1-4) 무한 재부팅
- 이 문제는 클라이언트의 부팅이 진행 되나, 윈도우 로딩화면에서 넘어가지 않고 블루스크린이 발생하여
   다시 부팅을 하는 과정이 무한 반복되는 현상입니다.
- 디스크가 잘못 만들어졌을 때 이런 문제가 발생할 수 있습니다.
- 클라이언트 초기화 기능을 사용하여 해당 디스크를 재생성해서 해결합니다. (사진13)

![사진15, 윈도우 로딩 ](/images/troubleshooting_server/image15.png)

### 2) IP 주소 변경 실패 시 장애 대응
#### 2-1)관리자 페이지 값이 변경되지 않았을 때

- IP 수정시 다음 파일에서 자동으로 수정이 됩니다. 수동으로 파일이 수정 되었는지 확인 필요 합니다.
- Localserver -> /etc/network/interface
- Localserver -> /etc/dhcp/dhcpd.conf
- Localserver -> /Korearental_Diskless_Admin/Korearental_Diskless_Admin/definition.py  > localServerUrl
- Dbserver -> mysql diskless_admin/diskless_administrator_server

![사진16, 서버 관리](/images/troubleshooting_server/image16.png)

### 3) 데이터베이스 강제종료에 의한 클러스터 장애
#### 3-1) 데이터베이스 서버 1대만 살아 있을때

- 강제 종료된 컴퓨터를 부팅 하시면 됩니다.

![사진17, 클러스터 서버가 1대 만 살아 있고 나머지 다 강제종료가 되었을 때 생기는 문제](/images/troubleshooting_server/image17.png)

#### 3-2) 데이터베이스 서버 모두다  강제 종료로 생기는 문제
- 한 개의 서버 선택 하여 다음 명령어실행
- vi /var/lib/mysql/grastate.dat
- safe_to_bootstrap: 1로 변경 한다.
- 나머지 서버를 재 시작 한다.

![사진18, 모든 클러스터 서버가 강제종료로 생긴 문제](/images/troubleshooting_server/image18.png)
