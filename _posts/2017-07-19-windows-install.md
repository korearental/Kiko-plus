---
layout: post
title: "윈도우즈 설치 및 설정 가이드"
description: "윈도우즈 설치 및 설정 가이드"
date: 2017-07-19
tags: [install, guide, dess, windows]
comments: true
share: true
---


## 0. 설치 환경 준비

* 윈도우 인스톨 기능을 사용하기 전에 클라이언트 등록을 해야 합니다. (클라이언트 등록 매뉴얼 참조)
* 설치 가능한 OS는 windows 7, window10에 한정됩니다.
* 설치 전 해당 OS의 인스톨 USB를 준비해야 합니다.


## 1. 윈도우 설치
### 1) Boot mode (win10)
- 상위 설정은 win10 이미지를 로컬 업로드 방식으로 전송 하기 위한 설정입니다. 
- 리모트 업로드로 이미지 업로드 하면 상위 설정 하지 않아도 됩니다.

![Boot mode 설정](/images/windows_install/image17.png)

### 2) 클라이언트 부팅 순서 설정 (BIOS)
- 준비해둔 윈도우 인스톨 USB를 클라이언트에 꽂은 뒤에 클라이언트 PC를 켜서 BIOS 셋업으로 들어갑니다.
- 부팅 순서에서 첫 번째는 USB부팅으로 설정한 뒤 저장하고 재 부팅합니다.

![부팅 순서 설정](/images/windows_install/image5.jpeg)


### 3) 네트워크 드라이버 초기화
- 설치 시 파티션 옵션에서 디스크파티션을 원하는 사이즈 만큼 지정(100G)합니다. 
- 나머지 공간도 할당을 합니다.(파티션 총 3개가 생성됨 ) 
- 위의 그림에서와 같이 디스크0파티션1:시스템 예약 파티션이 생성 됩니다. 
- 디스크0파티션1(윈도우 인스톨 할 위치 파티션) 을 삭제 하고  시스템 예약된 파티션을 확장을 합니다. 
- 2개의 파티션이 남게 되며 디스크0파티션2를 삭제 하여 “할당되지 않은 공간“ 으로 만듭니다. 
- 최종적으로 디스크0파티션 1 만 있고, 나머지 공간은 할당 하지 않아야 합니다.

![파티션 설정](/images/windows_install/image6.png)



## 2. 윈도우 설정

### 1) 슬립모드 해제(win7)
- 윈도우가 켜지면 제어판->하드웨어 및 소리->전원 옵션을 실행합니다.
- 하단의 ‘균형 조정’ 옵션 옆에 있는 ‘설정 변경’ 항목을 선택합니다.
![전원옵션](/images/windows_install/image9.png)

- 디스플레이 끄기, 컴퓨터를 절전 모드로 설정 항목을 해당 없음으로 설정합니다.
- ‘하단의 고급 전원 관리 옵션 설정 변경’ 항목을 선택합니다.

![설정변경](/images/windows_install/image10.png)

- 하드 디스크->다음 시간 이후에 하드 디스크 끄기의 값을 0으로 변경합니다.
- 절전 -> 다음 시간 후 절전 모드로 전환의 값을 0으로 변경합니다.
- 절전 -> 하이브리드 절전 모드 허용의 값을 해제로 변경합니다.
- 절전 -> 다음시간 이후에 최대 절전 모드로 전환의 값을 0으로 변경합니다.
- 디스플레이 -> 다음 시간 이후에 끄기의 값을 0으로 변경합니다.
- 상기의 작업들을 ‘균형 조정’ 옵션뿐 아니라 ‘절전’, ‘고성능’ 등 다른 모든 전원 관리 옵션에 동일하게 적용합니다.

![고급 전원관리 옵션](/images/windows_install/image11.png)

### 2) 에이전트 설치 (win7, win10)
- 윈도우 7, 10 버전에 따라 실행 에이전트 선택
![에이전트 선택](/images/windows_install/image12.png)

### 3) 셧다운매니저 등록

- c 드라이브에 Shutdown manager 폴더 안에 있는 Shutdownmanager.exe.config 파일을 메모장으로 실행합니다.
- 중간에 있는 아이피 주소를 마스터 서버의 주소 값으로 변경합니다.
- PC를 종료합니다.

![shutdownmanager.exe.cfg](/images/windows_install/image13.png)

## 3. 윈도우 이미지 업로드

### 1) BIOS 설정

- 준비해둔 Ubuntu live CD USB를 클라이언트에 꽂은 뒤에 클라이언트 PC를 켜서 BIOS 셋업으로 들어갑니다.
- 부팅 순서에서 첫 번째는 USB부팅으로 설정한 뒤 저장하고 재 부팅합니다.

![부팅순서설정](/images/windows_install/image5.jpeg)

### 2) ubuntu live CD 부팅

- 부팅 후 화면에서 첫 번째 줄을 선택한다.

![부팅순서설정](/images/windows_install/image14.png)

### 3) 원격 업로드

- 아래 명령어 실행후 [1] Virtual Disk Remote Upload를 선택한다.
- #>python /Diskless_Installer/Addon/vdu.py

![원격 업로드 실행-1](/images/windows_install/image15.png)

- Source Disk : /dev/sda1 입력
- Server IP Address : Local Server IP 입력
- Master’s Name : Image 명 입력


### 4) 로컬 업로드

-아래 명령어 실행 후 [2] Virtual Disk Local Upload를 선택한다.
- #>python /Diskless_Installer/Addon/vdu.py

![원격 업로드 실행-1](/images/windows_install/image16.png)


- Source Disk : /dev/<윈도우 하드 위치> 입력 ( 서버의 하드의 개수에 따라 입력 값이 변경 됩니다. )
- Master’s Name : Image 명 입력


## 4. 윈도우 이미지 활성화 (서버)

- Master1을 사용하는 클라이언트를 생성합니다.
- ＂클라이언트 업데이트＂을 활성화하고 바로 비활성화 합니다.
- ＂클라이언트 업데이트＂ 기능으로 마스터 서버로만 전송된 윈도우 이미지는 모든 서버에 동기화되고 모든 클라이언트
이미지를 재생성 합니다.

![클라이언트 업데이트](/images/windows_install/image20.png)
