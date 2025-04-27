---
title:  "하이퍼바이저(Hypervisor)"
categories: tech
tags:
  - hypervisor
toc: true
comments: true
---
이 Post에서는 하이퍼바이저(Hypervisor)에 대해 간략히 알아보려고 한다.
많은 사람들이 가상화, 클라우드란 용어는 이제는 익숙하게 느껴지지만
하이퍼바이저란 용어는 상대적으로 생소하게 다가올 수 있다. 하지만 가상화와
클라우드 컴퓨팅이 현실적으로 가능하게 해준 핵심 기술이 하이퍼바이저이다.
이 Post를 통해 하이퍼바이저 기술에 대한 이해도가 높아지길 바란다.

## 하이퍼바이저란?

+ Virtual Machine Manager(VMM)이라고도 불리며 하드웨어 가상화(Hardware
  Virtualization) 기술 중 하나로 여러 개의 OS들이 하나의 호스트 머신(Host
  Computer)위에서 돌게 해준다.

![Hypervisor](https://www.dropbox.com/s/qgchb6vpqt016jn/hypervisor.jpeg?raw=1)

[image source: <https://developer.ibm.com/tutorials/l-hypervisor/>]

## 하드웨어 가상화의 장점 (Advantages of platform virtualization)

### 머신 합병(machine consolidation)

U.S. EPA 연구에 의하면 서버 용량의 약 5%만 실제로 사용된다고 한다. 단일
서버에서 여러 플랫폼을 가상화하면 서버 활용도를 높일 수 있고 활용도가
높아지므로 서버 수가 줄어드는 효과를 가지고 온다.

### 데이터센터 유지비용 감소

서버 수가 줄어들면 유지비용 및 전력, 관리비용 절감할 수 있다. 또한 관리자에게
배정되는 서버 수가 줄어들기 때문에 안정성이 높아진다.

**Note**: 이렇든 저렇든 해도 가장 큰 장점은 **비용절감**이다.
조사에 의하면 하드웨어 가상화로 **50 ~ 70% overall IT 비용**을 줄인
사례들이 많다.
{: .notice--warning}

## 하드웨어 가상화 기술의 종류 (Types of hardware virtualization technologies)

### Guest Operating System Virtualization

가장 이해하기 쉬운 가상화 기술이다. 하드웨어 장비(보통 서버)에서 윈도우,
리눅스, 유닉스, 맥OS와 같이 수정되지 않은 원본 OS 이미지, 즉 Guest OS를
구동한다. 이 수정되지 않은 OS들을 구동하는 것은
virtual application (가상화 어플)이 담당하고 이 virtual application안에서
Guest OS를 돌리기 위한 VM(가상화 머신)들이 생성된다.

이 virtual application은 각각의 VM들의 시작, 종료, 관리등을 책임진다. 특히
물리적 하드웨어의 리소스에 대한 접근을 각각의 VM 들을 대신하여 관리한다.
또한 이 virtual application은 각각의 VM들이
실행하는 명령어들을 관찰하다가 특정한 명령어(VM들간의 충돌이 야기될 수 있는
명령어 및 virtual application 영역의 명령어등등)들이 실행되었을 때 해당
명령어의 바이너리들을 안전한 명령어로 교체하는 **binary rewriting**을
수행하므로서 마치 각각의 VM들이 시스템 하드웨어에서 직접 구동되는 것처럼 느끼게
해준다.

![Guest Operating System
Virtualization](https://www.dropbox.com/s/jlzegmqs10h5d88/guest_os.jpeg?raw=1)

[image source: <https://www.virtuatopia.com/index.php?title=An_Overview_of_Virtualization_Techniques>]

+ **Guest Operating System Virtualization 사용한 기술**
  + [VMware](https://www.vmware.com/kr.html)
  + [VirtualBox](https://www.virtualbox.org/)
{: .notice--info}

### Shared Kernal Virtualization

Shared Kernel Virtualization은 리눅스, 유닉스의 구조적 설계의 이점들을 취했다.
이 기술을 이해하기 위해서는 **Kernel**과 **Root File System**을
먼저 이해하는 것이 도움이 된다.

+ Kernel : 리눅스, 유닉스의 core는 Kernel(커널)이다. Kernel은 OS와 물리적
  하드웨어 간의 모든 상호 통신 및 관계를 관리한다.
+ Root File System : OS가 잘 기능할 수 있도록 Librarires, Files, Utils들을
  포함하고 있다.
Shared Kernel Virtualization에서는 Guest OS들이 각각 자기 자신의 Root File
System을 가지며 Host OS의 Kernel을 공유한다.

![Shared Kernel
Virtualization](https://www.dropbox.com/s/mwddff2hlbzmirh/shared_kernel.jpeg?raw=1)

[image source:
<https://www.virtuatopia.com/index.php?title=File:Shared_kernel_diagram.jpg>]

이런 방식의 가상화는 Kernel이 시스템 리부팅 없이 동적으로 현재 Root File
System을 변경할 수 있다는 것(리눅스, 유닉스의 chroot)에 기반한다. **이런 가상화의
최대 단점은 Guest OS가 Shared Kernel의 버전과 호환되어야 한다는 것이다.** 단적인
예로 Linux Kernel기반 Shared Kernel에서는 MS Windows Guest OS를 구동할 수 없다.

+ **Shared Kernel Virtualization 사용한 기술**
  + [Linux-Vserver](http://www.linux-vserver.org/Welcome_to_Linux-VServer.org)
  + [Solaris Containers](http://www.oracle.com/technetwork/server-storage/solaris/containers-169727.html)
  + [OpenVZ](https://openvz.org/)
  + FreeVPS
{: .notice--info}

### Kernel Level Virtualization

Kernel Level Virtualization는 OS가 여러개의 VM들을 제어, 관리하기 위해
설계된 확장기능들을 포함하고 있는 수정된 Kernel위에서 구동된다. 위의 Shared
Kernel Virtualization과 달리 각각의 Guest OS들은 자기 자신의 Kernel안에서
구동된다. 하지만 Guest OS들이 같은 커널로 컴파일되어야 한다는 제한은 Shared
Kernel Virtualization과 비슷하다.

![Kernel Level
Virtualization](https://www.dropbox.com/s/y3dh6o30kjsvv0p/kernel_level.jpg?raw=1)

[image source: <https://www.virtuatopia.com/index.php?title=An_Overview_of_Virtualization_Techniques>]

+ **Kernel Level Virtualization 사용한 기술**
  + [User-mode Linux](http://user-mode-linux.sourceforge.net/)
  + [Linux KVM](https://www.linux-kvm.org/page/Main_Page)
{: .notice--info}

## 하이퍼바이저 구분 (Hypervisor Classification)

### Type1 (native)

하드웨어 위에 하이퍼바이저가 있고 그 위에 Guest OS들이 구동되는 형태

+ Kernel Level Virtualization

### Type2 (host)

하드웨어 위에 Host OS가 있고 그 위에 하이퍼바이저와 Guest OS들이 구동되는 형태

+ Guset OS Virtualization, Shared Kernel Virtualization

![Hypervisor Type](https://www.dropbox.com/s/81o9mskew68y6ms/hypervisor_type.jpeg?raw=1)

[image source: <https://en.wikipedia.org/wiki/Hypervisor>]


## Type1 하이퍼바이저 가상화기술

### Protection Rings

Computer Science에서 Protection Rings는 데이터, 장애로 기능을 상실하는 것,
유해한 행동에서 보호하기 위한 매커니즘이다. x86군 CPU에서는 Protection rings가
실행될 수 있는 보호레벨들의 범위를 제공하는데 Ring 0이 가장 높은 레벨이며 보통
Kernel이 Ring 0에서 구동된다. OS위에서 구동되는 어플들은 일반적으로 Ring 3에서
코드가 실행된다. Guest OS의 Kernel들은 통상 Ring 0에 위치하는데 하이퍼바이저
가상화에서는 하이퍼바이저가 Ring 0에 위치하므로 Guest OS의 kernel들은 Ring
0보다 낮은 레벨에 위치하여야 한다. 하지만 대부분의 Guest OS의 kernel들은 CPU
특수명령 실행, 직접적으로 메모리 조작과 같이 Ring 0에서만 가능한 일들을
Ring 0에서만 수행하도록 만들어졌다. 이러한 문제들을 해결하기 위한 방안이 최근에
고안되었고 그 방법은 크게 Para-virtualiczation과 Full Virtualization으로
나뉜다.

![Protection
Rings](https://www.dropbox.com/s/5v6v4ozq6zgz4d6/x86_rings.png?raw=1)

[image source: <https://en.wikipedia.org/wiki/Protection_ring>]

### Para-virtualization

Para-virtualization에서는 Guest OS Kenrel이 Ring 0에 위치한 하이퍼바이저보다
낮은 보호레벨에서 구동될 수 있도록 Guest OS Kernel이 수정된다(Modified Guest
OS System). 이해를 돕기 위해 약간 풀어쓰면 Guest OS Kernel안의 Ring 0에서만
돌게되어 있는 동작들이 있는데 이를 하이퍼바이저를 통하여 동작할 수 있도록
수정하는 것이다. Guest OS Kernel을 수정해야 하기 때문에 이 기법은 Linux와 같이
오픈소스 OS에 주로 쓰인다.

![Para-virtualization](https://www.dropbox.com/s/m0u28rwvsph36b6/para_virtualization.jpeg?raw=1)

### Full Virtualization

Full Virtualization은 수정되지 않은 Guest OS들을 지원한다. Guest OS가 수정되지
않았기 때문에 Ring 0에서만 작동하는 동작들을 가지고 있으나 Full
Virtualization에서는 CPU Emulation을 통하여 이런 동작들을 제어하고 보호한다.
이런 CPU Emulation 프로세스가 Para-virtualization에 비해 시간과 자원을
소모하므로 상대적으로 느리다는 단점을 갖고 있다.

![Full
Virtualization](https://www.dropbox.com/s/5hgwq081eiohe3v/full_virtualization.jpeg?raw=1)

### Para-virtualization과 Full Virtualization의 성능비교

  +[성능비교](
  http://shortrecipes.blogspot.kr/2009/03/xen-performance-of-full-virtualization.html
  )

---

### Reference

+ [An Overview of Virtualization Techniques](http://www.virtuatopia.com/index.php/An_Overview_of_Virtualization_Techniques)
+ [Anatomy of a Linux hypervisor](http://www.ibm.com/developerworks/linux/library/l-hypervisor/)
+ [Protection rings](https://en.wikipedia.org/wiki/Protection_ring)
