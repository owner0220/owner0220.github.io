---
title: VirtualBox 네트워크 설정
date: 2021-04-14 09:29:00
categories:
 - NETWORK
 - VIRTUALBOX
tag:
 - NETWORK
 - VIRTUALBOX
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

- VM > Settings > Network
- File > Preferences > Network









