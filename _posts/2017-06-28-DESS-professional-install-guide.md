---
layout: post
title: "DESS Professional Edition 설치 가이드"
description: "DESS Pro Edition 설치 가이드 "
date: 2017-06-28
tags: [install, guide, dess, professional]
comments: true
share: true
---
목 차
=====

[*1.* *개요* ](#1-개요)

- [*1.1* *용어 정의* ](#11-용어-정의)

- [*1.2* *제약 사항* ](#12-제약-사항)

[*2.* *준비물* ](#2-준비물)

- [*2.1* *환경 설정* ](#21-환경-설정)

- [*2.2* *우분투 설치 USB* ](#22-우분투-설치-usb)

[*3.* *서버 설치 개요* ](#3-서버-설치-개요)

- [*3.1* *서버 구성* ](#31-서버-구성)

- [*3.2* *설치 순서* ](#32-설치-순서)

[*4.* *DB 서버 설치* ](#4-db-서버-설치)

- [*4.1* *우분투 설치* ](#41-우분투-설치)

- [*4.2* *DBMS 설치* ](#42-dbms-설치)

- [*4.3* *MMS(=local server)로의 접근권한 부여* ](#43-mmslocal-server로의-접근권한-부여)

[*5.* *BMS 서버 설치* ](#5-bms-서버-설치)

- [*5.1* *우분투 운영체제 설치* ](#51-우분투-운영체제-설치)

- [*5.2* *BMS 서비스 요소 설치* ](#52-bms-서비스-요소-설치)

- [*5.3* *설치 페이지 열기* ](#53-설치-페이지-열기)

- [*5.4* *설치 페이지 실행* ](#54-설치-페이지-실행)

[*6.* *MMS 서버 (=local server) 설치* ](#6-mms-서버-local-server-설치)

- [*6.1* *파티션 설정* ](#61-파티션-설정)

- [*6.2* *우분투 설치* ](#62-우분투-설치)

- [*6.3* *Installer 실행* ](#63-installer-실행)

[*7.* *Q & A* ](#7-q--a)

- [*7.1* *MMS(local server) 설치 시 오류 발생하는 경우* ](#71-mmslocal-server-설치-시-오류-발생하는-경우)

- [*7.2* *디스크 파티션 설정 오류* ](#72-디스크-파티션-설정-오류)

## 1. 개요

### 1.1  용어 정의

-   DESS (Diskless Enterprise System Suite) Basic Edition 의 설치에 앞서 용어의 혼선을 피하기 위하여 미리 용어를 정의하여 후술하는 설치 가이드 문서는 이를 따르기로 한다.

-   DESS: Diskless Enterprise System Suite 의 약자로서, Diskless 시스템의 공식 명칭으로서 후술하는 본 시스템을 줄여서 부르기로 함

-   노드: DESS 시스템 세트 한 개당 한 개의 노드를 구성한다. IT 인프라 수요처가 여러 개의 노드로 구성하여 도입하는 경우 DESS 시스템과 혼동을 피하기 위하여 DESS 개별 독립 망을 구분하여 칭하기로 함

-   MMS: Main Management Server 의 약자로서, DESS 시스템 내부를 구성하는 다수 서버의 일종이다. Basic 에디션에서는 구분하지 않으며, 스탠다드 이상 에디션에서 사용된다. 주로 스토리지 관리를 담당하는 서버 요소 임

-   BMS: Boot Management Server 의 약자로서, DESS 시스템 내부를 구성하는 다수 서버의 일종이다. Basic 에디션에서는 구분하지 않으며, 스탠다드 이상 에디션에서 사용된다. 주로 클라이언트 PC의 부팅 제어를 관리하는 서버 요소 임

-   DBMS: DB 서버로 통칭하여 부르기도 함. 지속적인 시스템 데이터를 저장하고 관리하는 서버 요소로서, 스탠다드 이상 에디션에서 따로 서버 하드웨어로 분리하여 사용함

-   CMS: Central Management Server 의 약자로서, 선택적으로 설치가 가능한 서버 요소이다. 여러 개의 노드를 관리하기 위한 목적으로 사용된다.

-   클라이언트: 1개의 노드에 접속하는 개별 PC를 서버와 구분하여 클라이언트라고 부르기로 한다. PC라고 할 때, 하드웨어적인 의미가 있기 때문에 혼동이 발생할 수 있어 “클라이언트 PC” 또는 클라이언트라고 통칭하기로 한다.

-   에이전트: Agent, 클라이언트에 설치되는 유틸리티 소프트웨어로서 주로 클라이언트의 상태 (네트워크 연결 여부)를 체크하기 위한 용도로 초기 OS 설치 시 함께 깔리게 된다.

### 1.2  제약 사항

-   DESS 스탠다드 에디션은 2개의 H/W 서버에 모든 구성요소가 집약 되어 있는 형상이다.

-   시스템 실패 시 재부팅 하는 등의 조치를 취해야 한다.

-   데이터 백업을 선택적으로 실시하여 하드웨어 고장에 대비해야 한다.

-   클라이언트 운영체제의 라이선스는 책임지지 않는다.

-   특정 클라이언트 H/W 호환성이 문제가 되는 경우 부팅이 실패할 수 있다.

## 2. 준비물

### 2.1 환경 설정

-   로컬서버 설치를 수행함에 앞서 적합한 네트워크 환경을 조성해야 한다.

-   스위치 장비는 NAT 기능이 포함된 것을 권장하며, 외부 네트워크와 분리하여 망이 운영될 수 있도록 한다.

-   망 분리가 어려운 경우 외부 DHCP 서버의 영향을 받지 않도록 네트워크를 구성하도록 주의해야 한다. (디스크리스 서버 (또는 BMS) 에 자체적으로 내장된 DHCP 서버가 응답하기 전에 외부 DHCP 서버가 응답을 하는 경우, 네트워크 부팅이 실패하게 된다.)

-   네트워크 케이블 선은 CAT-6 이상 사양을 권장하며, 스위치, 클라이언트의 네트워크 카드(NIC) 의 사양도 1Gbps 이상이 되도록 조건을 구비해야 원활한 작동성을 보장할 수 있다.

-   한편, L2/L3 장비의 포트 패스트 기능으로 패킷 드롭이 되는 현상이 발생할 수 있으니, 시스템 관리자와 문의하여 확인이 필요하다.

<img src="/images/install_standard/media/image1.png" width="565" height="424" />

### 2.2 우분투 설치 USB

-   전달 받는 iso 파일은 부팅가능한 usb로 만든다.

-   USB 종류에 따라 이미지 굽는 것이 실패할 수도 있으니 Sandisk 를 추천하며, 이미지를 USB로 굽는 소프트웨어는 unetbootin 소프트웨어를 사용하는 것을 추천한다. ([*https://unetbootin.github.io/*](https://unetbootin.github.io/))

-   또는 다음 사이트를 참고 ([*https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows*](https://tutorials.ubuntu.com/tutorial/tutorial-create-a-usb-stick-on-windows))


## 3. 서버 설치 개요

### 3.1 서버 구성

1.  BMS 서버

    -  클라이언트 부팅 관리 서버

    -  내부적으로 DHCP, TFTP 서비스가 구동됨

    -  관리 웹 서비스 구동됨

2.  DBMS 서버

    -  DB 관리 서버

    -  이중화 하지 않음

3.  MMS 서버

    -  클라이언트 OS 이미지 저장 및 관리 서버

    -  내부적으로 원격 스토리지 서비스가 구동됨

    -  이중화 구성하여 HA, LB 지원


### 3.2 설치 순서

1.  데이터베이스(DBMS) 설치

    1)  DB 서버 리눅스 운영체제 설치

    2)  DBMS 설치 및 설정

2.  BMS 서버 설치

3.  MMS 서버 설치

    1)  MMS 서버 리눅스 운영체제 설치

    2)  MMS 서비스 설치

    3)  DBMS 와 연동 구성 설정

4.  연동 구성 및 테스트

## 4. DB 서버 설치


-   <span id="_Toc479343209" class="anchor"></span>서버 설치는 리눅스 운영체제 위에 동작하도록 설계 되어 있어, 이 글을 읽고 있는 엔지니어는 리눅스 운영체제에 대해 어느 정도 지식을 가지고 있는 것을 가정하기로 한다. (이에 대해서는 다음 사이트를 참고함 - https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop)

-   또한 리눅스 사용자 가이드([*https://help.ubuntu.com/stable/ubuntu-help/*](https://help.ubuntu.com/stable/ubuntu-help/))를 참고하여 서버 관리에 활용할 수 있도록 추천함

-   DESS Standard 구성에서는 Basic 과 달리 서버 3-4대로 분리하여 운영되는 시스템이기 때문에, 리눅스 운영체제를 3번 설치하는 과정이 다른 점이다.

-   각각 리눅스 운영체제를 설치하고, 한 곳에는 DBMS 를 설치하고, 다른 한곳에는 BMS를 설치하며 나머지 다른 서버에는 MMS 서비스(‘로컬 서버’ 라고도 부른다) 를 설치하는 과정을 거치게 되는데, 먼저 DBMS 설치 과정을 기술하기로 한다.

### 4.1 우분투 설치


1)  부팅 USB를 사용하여 우분투 설치 환경으로 부팅한다.

2)  BIOS 설정에 부팅 순서가 USB 부팅이 1순위로 되어야 한다. (HDD나 CD 등이 1순위면 부팅이 정상적으로 되지 않는다.)

3)  Install Ubuntu를 선택한다. (실제 디스크에 설치하는 과정)

<p align="center">
<img src="/images/install_professional/media/image2.png" width="600" />
</p>

4)  와이파이 모양 아이콘 -&gt; Edit Connections를 누른다. (Ethernet 리스트는 하드웨어 스펙에 따라 상이함)

<p align="center">
<img src="/images/install_professional/media/image3.png" width="600"  />
</p>

5)  Wired connection 1을 선택하고 Edit 버튼을 누른다.

6)  Editing Wired connection 1 창에서 IPv4 Settings 탭으로 이동한다.

<p align="center">
<img src="/images/install_professional/media/image4.png" width="600" />
</p>

7)  Method를 Manual로 변경하고 Add버튼을 눌러 Address, Network, Gateway, DNS servers를 입력한다. (아이피 정보는 네트워크 환경에 따라 상이함)

<p align="center">
<img src="/images/install_professional/media/image5.png" width="600"  />
</p>


8)  저장 버튼을 눌러 인터넷에 연결되는지 체크한다.

9)  인터넷이 연결되었다면 Continue 버튼을 누른다.

<p align="center">
<img src="/images/install_professional/media/image6.png" width="600" />
</p>

10)  Something else를 누르고 Continue 버튼을 누른다.

<p align="center">
<img src="/images/install_professional/media/image7.png" width="600" />
</p>

11)  우분투를 설치 할 /dev/sda1(100G)를 선택하고 Change를 누른다.

12)  Edit partition 창에서 Use as는 ext4, Mount point는 /로 한다.

13)  /dev/sda1을 제외한 나머지 파티션은 기억한다. (/dev/sdb와 같이 숫자로 끝나지 않는 것은 뺌)

<p align="center">
<img src="/images/install_professional/media/image8.png" width="600" />
</p>

14)  /dev/sda1을 선택하고 인스톨을 시작한다.

15)  이어서 나오는 Do you want to return to the partitioning menu? / Do you want to return to the partitioner? 에서는 모두 Continue를 선택한다.

<p align="center">
<img src="/images/install_professional/media/image9.png" width="600" />

<img src="/images/install_professional/media/image10.png" width="600"  />
</p>

16)  이어서 나오는 지역 설정, 키보드 설정은 default 값으로 한다.

17)  Who are you? 에서는 사용자 계정을 만든다.

18)  사용자 계정은 우분투 로그인할 때 사용되니 반드시 기억한다.

<p align="center">
<img src="/images/install_professional/media/image11.png" width="600" />
</p>

19)  재 부팅 후 BIOS 설정에서 부팅 순서를 HDD 부팅이 1순위로 변경한다.

--------------------

### 4.2 DBMS 설치

1)  아래 명령어를 실행하여 설치 스크립트(Installer)를 실행 후 순서에 따른다.

2)  \# python /Diskless\_Installer/installer.py

3)  \[1\] Install Database 를 선택한다.

4)  이어서 \[1\] Install Database를 선택한다.

<p align="center">
<img src="/images/install_professional/media/image12.png" width="600" />
</p>

### 4.3 MMS(=local server)로의 접근권한 부여

1)  아래 명령어를 실행하여 설치 스크립트(Installer)를 실행 후 순서에 따른다.

2)  \# python /Diskless\_Installer/installer.py

3)  \[1\] Install Database 를 선택한다.

4)  이어서 \[2\] Grant Access to Local Server를 선택한다.

5)  Local server ip, 유저명, 암호를 입력한다.

<p align="center">
<img src="/images/install_professional/media/image13.png" width="600" />
</p>

## 5. BMS 서버 설치

-   BMS 서버의 설치 과정은 섹션 4 와 거의 동일하다.

-   4.1과 동일하게 우선 리눅스 운영체제를 설치하고, 이후 BMS 구성 요소 설치를 진행하여 완료하는 순으로 진행한다.

### 5.1 우분투 운영체제 설치

-   4.1 절과 동일

### 5.2 BMS 서비스 요소 설치


1)  아래 명령어를 실행하여 설치 스크립트(Installer)를 실행 후 순서에 따른다.

2)  \# python /Diskless\_Installer/installer.py

<p align="center">
<img src="/images/install_professional/media/image14.png" width="600" />
</p>

3)  \[2\] Install Diskless Server 를 선택한다.

4)  데이터베이스 서버 주소 입력 한다.

<p align="center">
<img src="/images/install_professional/media/image15.png" width="600" />
</p>

5)  로컬서버 사용자 이름 입력 한다.

### 5.3 설치 페이지 열기

1)  인터넷 창을 열어 localhost:8000 입력한다.


### 5.4 설치 페이지 실행

1)  주의사항

-   설치 도중에 브라우저의 뒤로 가기 버튼 또는 주소 창에 임의의 주소로 이동하지 마십시오.

-   도중에 실패하는 경우 처음부터 다시 실행해야 합니다.

2)  **설치 Step 1** &gt; Connecting Internet

<p align="center">
<img src="/images/install_professional/media/image16.png" width="600" />
</p>

3)  설치 Step 2&gt; Installing server

<p align="center">
<img src="/images/install_professional/media/image17.png" width="600"/>
</p>

4)  부팅 서버 입력란에 정보 입력 한다.

5)  로컬서버 입력란에 실제 로컬서버의 개수만큼 추가 하여 작성 한다.

<p align="center">
<img src="/images/install_professional/media/image18.png" width="600" />
</p>

6)  DHCP 정보 입력 한다.

<p align="center">
<img src="/images/install_professional/media/image19.png" width="600" />
</p>

7)  설치 준비 완료 되면 설치 시작 된다.

<p align="center">
<img src="/images/install_professional/media/image20.png" width="600" />

<img src="/images/install_professional/media/image21.png" width="600" />

<img src="/images/install_professional/media/image22.png" width="600" />

<img src="/images/install_professional/media/image23.png" width="600" />
</p>

## 6. MMS 서버 (=local server) 설치

-   서버 설치는 리눅스 운영체제 위에 동작하도록 설계 되어 있어, 이 글을 읽고 있는 엔지니어는 리눅스 운영체제에 대해 어느 정도 지식을 가지고 있는 것을 가정하기로 한다. (이에 대해서는 다음 사이트를 참고함 - https://tutorials.ubuntu.com/tutorial/tutorial-install-ubuntu-desktop)

-   또한 리눅스 사용자 가이드(<https://help.ubuntu.com/stable/ubuntu-help/>)를 참고하여 서버 관리에 활용할 수 있도록 추천함

-   DESS Professional구성에서는 Basic 과 달리 서버 여러 대로 분리하여 운영되는 시스템이기 때문에, 리눅스 운영체제를 여러 번 설치하는 과정이 다른 점이다.

### 6.1 파티션 설정

1)  앞서 설치 이미지를 제작한 USB로 부팅 시키면 Ubuntu 초기 설치 화면이 하단 이미지와 같이 표출된다.

2)  Try Ubuntu를 선택하여, PE (Pre Execution) 환경으로 진입한다.

-   운영체제를 디스크에 설치하지 전 단계로서 디스크의 파티션을 설정하는 단계를 미리 거치게 된다.

-   운영체제를 설치할 디스크를 미리 지정하는 작업이 수반된다.

<p align="center">
<img src="/images/install_professional/media/image24.png" width="600" />
</p>

3)  GParted Partition Editor를 실행한다.

    -   좌측 상단 우분투 로고 버튼을 누르고 gparted 라고 입력하면, Gparted Partition Editor 어플리케이션 아이콘이 띄워지고, 이를 선택하여 실행한다.

    -   이 어플리케이션은 파티션 작업을 GUI 환경에서 실행 할 수 있도록 지원하는 것으로서, 좀 더 자세한 설명을 원하면 다음 사이트를 참고 한다. ([*http://gparted.org/*](http://gparted.org/))

    -   동일한 작업을 콘솔로 수행할 수도 있는데, parted 란 명령어를 이용하여 작업하면 되는데, 이에 대한 설명은 다음 사이트를 참고한다. ( [*https://www.gnu.org/software/parted/manual/parted.html*](https://www.gnu.org/software/parted/manual/parted.html) )

    -   어플리케이션을 실행하면 서버에 장착된 디스크 (HDD, SSD 등) 장치들에 대해 작업을 진행할 수 있으며, 장치 별로 우 상단에서 선택할 수 있다. (리눅스에서는 장치 별로 /dev/sda, /dev/sdb 등과 같이 식별자가 자동으로 부여 되는 것에 유의하여 작업 대상이 되는 디스크가 어떤 것인지 확인하고 작업을 수행한다. 다만, 함께 전체 용량이 같이 볼 수 있으므로 잘못 선택하지 않도록 유의한다.)

    -   GParted Partition Editor 창의 메뉴에서 View -&gt; Device Information을 선택하면 아래와 같은 화면이 나타난다. (각 항목의 구체적인 수치는 하드웨어 스펙에 따라 상이함)

<p align="center">
<img src="/images/install_professional/media/image25.png" width="600" />
</p>

4)  Device -&gt; Create Partition Table을 실행하여 partition table 타입을 msdos로 변경한다.
    - 이는 /dev/sda, /dev/sdb 등 서버에 장착된 모든 디스크에 실행한다.

<p align="center">
<img src="/images/install_professional/media/image26.png" width="600" />
</p>

5)  /dev/sda 디스크에 우분투를 설치할 100G 파티션(filesystem은 ext4)을 할당한다.

<p align="center">
<img src="/images/install_professional/media/image27.png" width="600" />
</p>

6)  /dev/sda 디스크의 나머지 공간은 하나의 파티션으로 할당한다.

    -  /dev/sda 디스크에 파티션을 할당하면 아래와 같은 화면이 나타난다.

<p align="center">
<img src="/images/install_professional/media/image28.png" width="600" />
</p>

7)  /dev/sda를 제외한 서버에 장착된 나머지 디스크의 파티션은 통으로 할당한다.

<p align="center">
<img src="/images/install_professional/media/image29.png" width="600" />
</p>

8)  Apply를 눌러 지금까지의 파티션 변경사항을 저장한다.

<p align="center">
<img src="/images/install_professional/media/image30.png" width="600" />
</p>

9)  컴퓨터를 재부팅한다.


## 6.2 우분투 설치

1)  부팅 USB를 사용하여 우분투 설치 환경으로 부팅한다.

2)  BIOS 설정에 부팅 순서가 USB 부팅이 1순위로 되어야 한다. (HDD나 CD 등이 1순위면 부팅이 정상적으로 되지 않는다.)

3)  Install Ubuntu를 선택한다. (실제 디스크에 설치하는 과정)

<p align="center">
<img src="/images/install_professional/media/image2.png" width="600" />
</p>

4)  와이파이 모양 아이콘 -&gt; Edit Connections를 누른다. (Ethernet 리스트는 하드웨어 스펙에 따라 상이함)

<p align="center">
<img src="/images/install_professional/media/image3.png" width="600" />
</p>

5)  Wired connection 1을 선택하고 Edit 버튼을 누른다.

6)  Editing Wired connection 1 창에서 IPv4 Settings 탭으로 이동한다.

<p align="center">
<img src="/images/install_professional/media/image4.png" width="600" />
</p>

7)  Method를 Manual로 변경하고 Add버튼을 눌러 Address, Network, Gateway, DNS servers를 입력한다. (아이피 정보는 네트워크 환경에 따라 상이함)

<p align="center">
<img src="/images/install_professional/media/image5.png" width="600" />
</p>

8)  저장 버튼을 눌러 인터넷에 연결되는지 체크한다.

9)  인터넷이 연결되었다면 Continue 버튼을 누른다.

<p align="center">
<img src="/images/install_professional/media/image6.png" width="600" />
</p>

10)  Something else를 누르고 Continue 버튼을 누른다.

<p align="center">
<img src="/images/install_professional/media/image7.png" width="600" />
</p>

11)  우분투를 설치 할 /dev/sda1(100G)를 선택하고 Change를 누른다.

12)  Edit partition 창에서 Use as는 ext4, Mount point는 /로 한다.

13) /dev/sda1을 제외한 나머지 파티션은 기억한다. (/dev/sdb와 같이 숫자로 끝나지 않는 것은 뺌)

<p align="center">
<img src="/images/install_professional/media/image8.png" width="600" />
</p>

14) /dev/sda1을 선택하고 인스톨을 시작한다.

15) 이어서 나오는 Do you want to return to the partitioning menu? / Do you want to return to the partitioner? 에서는 모두 Continue를 선택한다.

<p align="center">
<img src="/images/install_professional/media/image9.png" width="600" />

<img src="/images/install_professional/media/image10.png" width="600" />
</p>

16) 이어서 나오는 지역 설정, 키보드 설정은 default 값으로 한다.

17) Who are you? 에서는 사용자 계정을 만든다.

18) 사용자 계정은 우분투 로그인할 때 사용되니 반드시 기억한다.

<p align="center">
<img src="/images/install_professional/media/image11.png" width="600" />
</p>

19) 재 부팅 후 BIOS 설정에서 부팅 순서를 HDD 부팅이 1순위로 변경한다.

### 6.3 Installer 실행

1)  설치 디렉토리에 접근권한 부여

2)  아래 명령어를 실행한다

<p align="center">
<img src="/images/install_professional/media/image31.png" width="600" />
</p>

3)  Installer 실행

<p align="center">
<img src="/images/install_professional/media/image32.png" width="600" />
</p>

4)  아래 명령어를 실행한다

5)  \#&gt; python /Diskless\_ISCSI\_Installer/iscsi\_installer.py

6)  부팅서버에서 설정하였던 로컬서버 이름 및 IP 값 입력한다.

<p align="center">
<img src="/images/install_professional/media/image33.png" width="600" />
</p>

7)  데이터 베이스 IP 주소를 입력 한다. (프록시 서버 추가 시 프록시 서버 주소 값 입력)

<p align="center">
<img src="/images/install_professional/media/image34.png" width="600" />
</p>

8)  디스크 풀 확인 맞으면 “Y” 입력한다.

<p align="center">
    <img src="/images/install_professional/media/image35.png" width="600" />
    <img src="/images/install_professional/media/image36.png" width="600" />
</p>

9)  설치 완료되면 컴퓨터 재부팅 한다.


## 7. Q & A

### 7.1  MMS(local server) 설치 시 오류 발생하는 경우

1)  아래 그림과 같이 명령어를 실행하여 Sql 연결상태를 확인한다.

  \#&gt; mysql -uroot -p -h ‘DB서버의 아이피 주소’

<p align="center">
<img src="/images/install_professional/media/image37.png" width="600" />
</p>

2)  Host ‘host의 ip 주소’ is not allow this MariaDB server 메시지 출력될 경우

3)  DB 서버에서 로컬 서버에 접근 권한을 부여 하지 않아 생긴 문제로 아래 순서대로 진행 한다..

<span style="color:red"> **하위 진행 하는 서버 는 DB 서버에서 진행 한다.** </span>

4)  아래 명령어를 입력하여 인스톨러 실행한다.

  \#&gt;python /Diskless\_Installer/installer.py

<p align="center">
<img src="/images/install_professional/media/image38.png" width="600" />
</p>

5)  \[1\] Install Database를 선택한다.

6)  \[2\] Grant Access to Local Server를 선택한다.
<p align="center">
<img src="/images/install_professional/media/image39.png" width="600" />
</p>

7)  local server’s ip address, username, and username’s password 를 입력한다.
<p align="center">
<img src="/images/install_professional/media/image40.png" width="600" />
</p>

8)  아래 명령어를 실행하여 DB연결상태를 재 확인한다.

  \#&gt; mysql -uroot -p -h ‘DB 서버의 IP주소’

<p align="center">
<img src="/images/install_professional/media/image41.png" width="600" />
</p>

9)  성공하면 위와 같은 화면이 출력되며 이어서 설치를 진행하면 된다.

### 7.2 디스크 파티션 설정 오류

1)  GPartition을 실행한다

<p align="center">
<img src="/images/install_professional/media/image42.png" width="600" />
</p>

2)  모든 파티션 테이블이 “msdos”로 설정되어 있는지 확인한다.

<p align="center">
<img src="/images/install_professional/media/image43.png" width="600" />
</p>
