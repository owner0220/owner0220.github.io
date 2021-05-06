---
title: VirtualBox 네트워크 설정
date: 2021-04-15 21:56:00
categories:
 - VIRTUALBOX
tag:
 - Network
---

### Virtualbox NETWORK 설정

하드웨어 가상화의 핵심 아이디어 중 하나는 물리적 컴퓨터도 사용할 수 있는 거의 모든 경우에 가상 컴퓨터를 사용할 수 있다는 것입니다.

<!-- more -->

#### Virtual Network Adapters

각각 VirtualBox VM은 8개의 가상 네트워크 어댑터를 사용할 수 있고, VM은 NIC(Network Interface Controller)를 가지고 있는데 GUI에서는 4개의 어댑터를 설정할 수 있지만, cli의 `VBoxManage modifyvm` 명령어를 통해서 총 8개까지 설정할 수 있습니다.

**VBoxManage** 로 조정하는 것은 GUI로 봤을 때 vm 을 클릭 하고 setting을 눌러 network를 세팅하는 것을 말합니다.

![Virtualbox_Setting](/assets/images/Virtualbox_Setting.PNG)



#### Virtualbox에서 제공하는 Network Adapters 종류

- AMD PCnet-PCI II (Am79C970A)
- AMD PCnet-FASt III (Am79C973)
- Intel PRO/1000 MT Desktop (82540EM)
- Intel PRO/1000 T Server (82543GC)
- Intel PRO/1000 MT Server (825456EM)
- Paravirtualized Network Adapter (virtui-net)

※ 업계 표준 virtIO 네트워킹 드라이버는 VirtualBox에서 지원됩니다. (VirtIO 네트워킹 드라이버는 KVM 프로젝트의 일부이며 오픈소스 입니다.)



##### Jumbo frames support

VirtualBox는 jumbo frames (Ethernet frames that can carry packets which size is more than 1,500 bytes)에 대한 제한된 지원을 제공합니다.

jumbo frames를 사용해야 할 경우

- intel virtualized network adapter는 bridged mode를 설정해 사용합니다.
- AMD-based virtual network adapter는 위 기능을 지원하지 않습니다.
  - 사용할 경우 입력 및 출력 트래픽에 대해 점보 프레임이 자동으로 삭제됩니다. (jumbo frames are disabled by default)



#### VirtualBox Network Modes

VirtualBox는여러 네트워크 모드를 제공합니다.(VM을 클릭, setting을 수정할 경우 VM의 어댑터가 연결될 네트워크와 VM의 네트워크 어댑터를 선택합니다.)

- Not attached
- NAT
- NAT Network
- Bridged Adapter
- Internal Network
- Host-only Adapter
- Generic Driver



##### Not attached

- VM에는 네트워크 어댑터가 설치 되어 있지만, 물리적으로 이더넷 케이블 뽑아 놓은 상황

##### NAT

- 네트워크 어댑터 기본 설정으로 인터넷은 되지만, 외부에서 VM에 접속은 안 됩니다.

- DHCP을 통해 (10.0.2.15) IP를 고정 할당 받습니다.

  (IP가 Virtualbox 에 built-in으로 고정되어 있습니다.)

- 각 VM의 기본 게이트웨이는 10.0.2.2 입니다.

- 네트워크 마스크 255.255.255.0 입니다.

※ 각각의 VM은 독립적인 NAT 디바이스로 구성되어 있습니다.

![Virtualbox_NAT](/assets/images/Virtualbox_NAT.PNG)

###### NAT 모드 활성화 cli 명령어

```bash
VBoxManage modifyvm VM_name --nic1 nat
```

*VM_name* : 가상 기기 이름

*nic1* : 가상 네트워크 어댑터 갯수

*nat* : 임의로 설정할 VirtualBox 네트워크 모드의 이름

​		[ none|null|nat|natnetwork|bridged|intnet|hostonly|generic ]

- 사용하지 않음                         [none]

- Not attached                          [null]
- NAT	                                      [nat]
- NAT Network                          [natnetwork]
- Bridged Adapter                    [bridged]
- Internal Network                   [intnet]
- Host-only Adapter                 [hostonly]
- Generic Driver                        [generic]

##### NAT Network

![Virtualbox_NAT_Network](/assets/images/Virtualbox_NAT_Network.PNG)

여러 VM에서 NAT Network를 사용할 경우 VM들은 NAT 네트워크를 통해서 VM들 끼리 통신이 가능하며, VM은 host 기기의 Physical NIC를 통해 외부 네트워크와 통신이 됩니다.

반면, 외부 네트워크의 모든 시스템과 호스트 시스템이 연결된 물리적 네트워크의 시스템은 NAT Network를 사용하도록 구성된 VM에 액세스 할 수 없습니다. (홈 네트워크에서 인터넷 액세스를 위해 라우터를 구성하는 경우와 비슷합니다.)

VirtualBox에서 NAT Network를 추가 구성할 수 있습니다.

![Virtualbox_NAT_Setting](/assets/images/Virtualbox_NAT_Setting.PNG)



![Virtualbox_NAT_Setting2](/assets/images/Virtualbox_NAT_Setting2.PNG)

default NatNetwork 구성 스위치 IP CIDR 구성은 10.0.2.0/24 입니다.

해당 관리 gateway IP는  10.0.2.1 (구성했던 CIDR 형태에서 x.x.x.1 형태로 IP는 default Gateway IP로 할당 됩니다.)

ex) CIDR 192.168.22.0/24

-> Gateway IP는 : 192.168.22.1으로 설정됩니다.

-> DHCP Server : 192.168.22.3 으로 설정됩니다.

※ NAT Network에서 사용되는 네트워크 게이트웨이 IP 주소는 변경할 수 없으며 DHCP 서버에서 발급한 IP 주소 범위는 변경할 수 없습니다. (마찬가지로 DHCP 서버 IP 주소는 기본적으로 x.x.x.3 형태로 할당 됩니다.)

###### 위에서 설정했던 설정을 cli로 설정할 경우

```bash
$ VBoxManage natnetwork add --netname natnet1 --network "192.168.22.0/24" --enable
```

*natnet1* : NAT 네트워크 이름

192.168.22.0/24  : NAT 네트워크에 설정할 CIDR



###### VM 어댑터에 NAT Network를 붙이도록 설정할 경우

```bash
$ VBoxManage modifyvm VM_name --nic1 natnetwork
```

*VM_name* : 설정을 세팅할 VM

*nic1* : 첫번째 VM 네트워크 어댑터

*natnetwork*  : 사용할 Virtualbox network mode

※ VM 에 위의 세팅을 적용할 경우 VM을 restart 시켜줘야 할 수 있습니다.

- 윈도우 6.0.12 r133076 GUI 로 수정 테스트 했을 때는 VM restart 없이 적용 되었습니다.



#### Port Forwarding

- VM > Settings > Network : VM에서 사용할 NIC 어댑터와 연결할 네트워크에 대한 설정
- File > Preferences > Network : NAT network를 설정, 추가 할 수 있는 설정



##### Bridged Adapter

VM의 가상 네트워크 어댑터를 VirtualBox 호스트 컴퓨터의 물리적 네트워크 어댑터가 연결된 물리적 네트워크에 연결하는 데 사용됩니다. VM 가상 네트워크 어댑터는 네트워크 연결에 호스트 네트워크 인터페이스를 사용합니다. 간단히 말해 네트워크 패킷은 추가 라우팅 없이 가상 네트워크 어댑터와 직접 주고 받습니다.

로컬 물리 네트워크에 접속하기 위해 주로 사용하며, VM에서 host, host와 연결된 네트워크, 외부 네트워크에 접속이 가능합니다. 이때 호스트에서 여러개의 물리 어댑터를 사용할 경우 사용할 어댑터를 설정해 주어야 합니다.

![Virtualbox_BridgedAdapter](/assets/images/Virtualbox_BridgedAdapter.PNG)

Bridged Adapter를 사용할 경우 VM 가상 네트워크 어댑터의 IP 주소는 호스트 컴퓨터의 물리적 네트워크 어댑터 IP 주소와 동일한 네트워크에 속할 수 있습니다. 이때 물리적 네트워크에 DHCP 서버가 있는 경우 VM의 가상 네트워크 어댑터는 Bridged mode에서 자동으로 IP주소를 얻습니다.

※ 게스트 OS의 네트워크 인터페이스 설정에서 자동으로 IP 주소 받기가 설정 된 경우에 가능

---

예를 들면

물리적 네트워크 주소 : 10.10.10.0/24

물리적 네트워크에 있는 기본 게이트웨이의 IP 주소 : 10.10.10.1

물리적 네트워크에 있는 DHCP 서버의 IP 주소 : 10.10.10.1

호스트 컴퓨터 IP 구성 : 10.10.10.72

넷 마스크 : 255.255.255.0

기본게이트웨이 : 10.10.10.1

게스트 컴퓨터의 IP 구성 : 10.10.10.91

넷 마스크 : 255.255.255.0

기본 게이트웨이 : 10.10.10.1

※ 따라서 bridge mode에서 작동하는 가상 네트워크 어댑터의 기본 게이트웨이는 호스트 컴퓨터와 동일합니다.

---

![Virtualbox_BridgedNetworking](/assets/images/Virtualbox_BridgedNetworking.PNG)

경우에 따라 물리적 네트워크에 여러 게이트웨이가 있는 경우가 있습니다. 하나의 게이트웨이를 통해 필요한 네트워크에 연결하기 위해 호스트 머신을 사용하고 두 번째 게이트웨이를 통해 다른 네트워크에 연결하기 위해 게스트 머신을 사용할 수 있습니다.

VM에서 라우팅 테이블을 편집하고 두 게이트웨이를 사용해 적절한 네트워크에 연결하기 위한 경로를 추가할 수도 있습니다.

**Promiscuous mode** (무작위 모드)

이 모드는 네트워크 어댑터가 지정되는 어댑터 주소에 관계없이 수신된 모든 트래픽을 전달할 수 있습니다. 일반 모드에서 네트워크 어댑터는 특정 네트워크 어댑터의 MAC 주소를 헤더의 대상 주소로 포함하는 프레임만 수신합니다. 	

이 모드는 물리적 네트워크 어댑터가 여러 MAC 주소를 가질수 있도록 할 수 있으며 모든 트래픽이 호스트 시스템의 물리적 네트워크 어댑터를 통과하고 호스트 어댑터에 표시되는 자체 MAC 주소가 있는 VM의 가상 네트워크 어댑터에 도달 할 수 있도록 합니다. IP가 설정되지 않은 VM 가상 네트워크 어댑터에도 도달됩니다.

무작위 모드 종류는 3개로 나뉘는데 

- Deny : VM의 가상 네트워크 어댑터로 보내지 않은 트래픽은 VM에서 숨겨집니다. (default)
- Allow VMs : 다른 VM 과 주고 받는 트래픽을 제외하고 모든 트래픽은 VM 네트워크 어댑터에서 숨겨집니다.
- Allow All : 제한이 없으며, VM 네트워크 어댑터는 모든 수신 및 발신 트래픽을 볼 수 있습니다.



#### Internal Network

내부 네트워크 모드에서 작동하도록 구성된 가상 머신은 격리된 가상 네트워크에 연결됩니다. 이 네트워크에 연결된 VM은 서로 통신할 수 있지만 VirtualBox 호스트 컴퓨터나 물리적 네트워크 또는 외부 네트워크의 다른 호스트와는 통신 할 수 없습니다. 내부 네트워크에 연결된 VM은 호스트 또는 다른 장치에서 액세스 할 수 없습니다. 주로 네트워크 모델링에 사용합니다.

예를 들어 각각 내부 네트워크에 연결된 가상 네트워크 어댑터(어댑터1)가 있는 세개의 VM을 만들 수 있습니다. 이러한 네트워크 어댑터의 IP 주소는 Virtualbox 내부 네트워크에 사용되는 서브넷에서 정의됩니다.(서브넷을 수동으로 정의해야 합니다.)

VM1 - (어댑터2) NAT mode 추가 설정 하여 라우터로 구성 (라우터 역할을 하기 위해 VM에 리눅스를 설치하고 iptables를 설치하는 것이 좋지만, 간단한 라우팅을 사용하기 위해서 내장된 기능을 사용한다.)

VM2, VM3 - 어댑터의 내부 네트워크 어댑터의 IP 주소가 있으면 외부 네트워크에 액세스 할 수있는 유일한 Virtual network에 접속되어 VM1은 네트워크 설정에서 게이트웨이로서 설정합니다.

구성으로 표현하자면

**VM1**

IP : 192.168.23.1 (내부 네트워크 모드), 10.0.2.15 (NAT 모드), 게이트웨이 10.0.2.2 (내장 VirtualBox NAT 장치 IP 주소)



**VM2**

IP : 192.168.23.2 (내부 네트워크 모드), 게이트웨이 192.168.23.1

**VM2**

IP : 192.168.23.3 (내부 네트워크 모드), 게이트웨이 192.168.23.1

VirtualBox 내부 네트워크 서브넷 192.168.23.0/24

![Virtualbox_InternalNetwork](/assets/images/Virtualbox_InternalNetwork.PNG)

※ 실제 네트워크 인프라에서 방화벽 규칙을 구현하기 전에 IPTABLES에서 방화벽 규칙을 테스트하기 위해 이러한 인프라를 배포 할 수도 있지만 외부네트워크에서 연결할 때 VM1의 두 번째 가상 네트워크 어댑터가 NAT 모드가 아닌 Bridge 모드를 사용하는 것이 좋습니다.



#### Host-only Adapter

호스트와 guest 간의 통신을 위해 사용됩니다. VM은 Host-only로 연결된 VM과 host에 통신이 가능합니다.

![Virtualbox_Hostonly](/assets/images/Virtualbox_Hostonly.PNG)

Host-only network를 사용하기 위해서는 호스트에서 해당 네트워크 어댑터를 생성해 주어야 합니다.  **File > Host Network Manager.**

![Virtualbox_hostnetwork](/assets/images/Virtualbox_hostnetwork.PNG)

위의 경우 Host-only network의 기본 네트워크 주소는 192.168.56.0/24 이고 호스트 컴퓨터에 있는 가상 네트워크 어댑터의 IP 주소는 192.168.56.1 입니다. 이러한 IP 주소는 수동으로 IP를 편집할 수 있습니다.

추가적으로 DHCP 서버 설정을 활성화/비활성화 할 수 있습니다. DHCP 서버 탭에서는 DHCP의 IP주소, 넷 마스크 및 DHCP 클라이언트에 대해 발급할  IP 주소 범위를 설정할 수 있습니다.

![Virtualbox_hostnetworkmanager](/assets/images/Virtualbox_hostnetworkmanager.PNG)

※ Host-only mode에서는 외부의 장치에서 호스트 전용 네트워크에 연결할 수 없기 때문에 VM의 가상 네트워크 어댑터에는 IP 구성에 게이트웨이가 없습니다.



#### Generic Driver

이 모드를 사용하면 일반 네트워크 인터페이스를 공유할 수 있습니다. 사용자는 확장 팩에 배포하거나 VirtualBox에 포함할 적절한 드라이버를 선택할 수 있습니다. 

- UDP Tunnel : 다른 호스트에서 실행되는 가상머신은 기존 네트워크 인프라를 사용하여 투명하게 통신할 수 있습니다.

- VDE Networking : 가상 머신은 Linux 또는 FreeBSD 호스트의 가상 Distributed Switch에 연결할 수 있습니다. 표준 Virtualbox 패키지에는 이 기능이 포함되어 있지 않으므로 VDE 네트워킹을 사용하려면 Virtualbox 소스에서 컴파일해서 만들어진 프로그램을 사용해야 합니다.



#### 네트워크 모드에 따른 접속 가능 확인

![Virtualbox_connectionbymode](/assets/images/Virtualbox_connectionbymode.PNG)



#### Port Forwarding

포트포워딩은 트래픽을 다른 IP 주소 혹은 포트로 리디렉션하는 것 외에도 적절한 IP 주소 및 포트로 주소가 지정된 트래픽을 가로채는 프로세스입니다.

컴퓨터 및 기타 라우터 장치에서 특수 응용프로그램을 사용하여 포트 전달을 구성할 수 있습니다.

포트 포워딩의 가장 일반적인 사용 사례 중 하나는 외부 네트워크에서 NAT 뒤에 숨겨진 특정 네트워크 서비스에 대한 액세스를 제공하는 것입니다. 포트포워딩 규칙을 구성한 후 클라이언트는 라우터 (호스트)의 외부 IP 주소 및 지정된 포트에 연결하여 외부에서 적절한 서비스에 액세스 할 수 있습니다.

패킷은 먼저 라우터의 응용 프로그램에서 가로채고 응용 프로그램은 해당 헤더 (IP 패킷 헤더, TCP 또는 UDP 세그먼트의 헤더)의 대상 IP 주소와 포트 번호를 읽습니다.

헤더의 대상 IP 주소 및 포트 번호 조합이 포트 포워딩 규칙과 일치하는 경우 라우팅 애플리케이션은 헤더 정보 (IP 정보 및 포트 번호)를 다시 쓰고 다른 네트워크 패킷 / 세그먼트로 보냅니다.



###### 포트 포워딩 적용 예시 (SSH 액세스)

호스트 IP : 10.10.10.72 (물리적 NIC)

VM IP : 10.0.2.15 (NAT mode)

사용자 이름 : user1

NAT로 구성된 VM 어댑터에서 Port Forwarding 규칙 설정

VM 클릭 >> Settings >> Network >> Adapter1

![Virtualbox_portforwarding](/assets/images/Virtualbox_portforwarding.PNG)

SSH 서버는 기본적으로 22 TCP 포트를 수신합니다.

포트 8022로 들어오는 연결을 VM 22번 포트로 전달 할 수 있는 규칙을 만들면 다음과 같습니다.

![Virtualbox_portforwardingrules](/assets/images/Virtualbox_portforwardingrules.PNG)

※ Host는 host, Guest는 guest

Virtualbox 호스트의 실제 네트워크 어댑터의 실제 IP 주소가 될 유사한 포트 전달 규칙을 만드는 경우 물리적 네트워크의 다른 호스트는 포트 8022에서 Virtualbox 호스트 시스템에 연결하여 SSH를 통해 VM에 액세스 할 수 있습니다.

이 예에서는 Virtualbox 호스트에 있는 물리적 NIC IP 주소는 10.10.10.72 입니다.

![Virtualbox_portforwardingrules2](/assets/images/Virtualbox_portforwardingrules2.PNG)

이렇게 되면 Virtualbox 호스트 또는 LAN에 연결된 다른 호스트에서 SSH 클라이언트를 열고 포트 8022에서 Virtualbox 호스트 IP에 연결합니다.



###### 포트 포워딩 적용 예시 (HTTP 액세스)

VM에 웹 서버를 배포하고 외부에서 웹 사이트에 대한 액세스를 제공하려는 경우 다른 포트 전달 규칙을 추가 할 수 있습니다.

Virtualbox 호스트 머신과 물리적 LAN (Local Area Network)에 연결된 다른 머신에서 VM에 배포된 웹 사이트에 액세스하기 위해 해당 포트 전달 규칙을 구성하는 방법을 예로 들면 다음과 같습니다.

먼저 웹 사이트가 정상 접속 되는지 확인한 후에 진행합니다. 웹은 TCP 8080  를 사용합니다.

VM settings > Network > select adapter > Port Forwarding

![Virtualbox_portforwardingrules3](/assets/images/Virtualbox_portforwardingrules3.PNG)

RDP, FTP 및 기타 프로토콜을 통해 VM에 액세스하기 위한 규칙을 정의할수도 있습니다.



#### 결론

Virtualbox는 유연하고 다양한 네트워크 설정을 제공하는 강력한 가상화 솔루션입니다. 

각 VM은 최대 8개의 가상 네트워크 어댑터를 사용할 수 있으며(단, GUI에서는 4개까지만 설정 가능) 각 네트워크 어댑터는 실제 Intel 및 AMD 네트워크 인터페이스 컨트롤러 (NIC)의 적절한 모델로 에뮬레이션 될 수 있습니다.