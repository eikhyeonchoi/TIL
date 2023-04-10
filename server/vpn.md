# AWS VPN
```
기본적으로 VPC는 온프레미스 네트워크와 통신할 수 없다
IPSec 프로토콜 스위트를 사용해 VPC와 온프레미스 네트워크 간의 프라이빗 네트워크를 구성하는데 이를 Site to Site VPN이라고 한다
여기서의 VPN이란 VPC와 온프레미스 네트워크 간의 연결을 의미함
```

## VGW - Virtual Private Gateway
```
VGW는 VPC와 온프레미스 네트워크를 연결하는 VPC쪽의 라우터 역할을 한다
VGW를 이용해 온프레미스 네트워크와 연결하기 위해서는 온프레미스 네트워크 쪽에 CGW - Customer Gateway를 구성해야함

특징
VGW는 여러 개의 온프레미스 라우터와 연결이  가능함
다만 VGW는 하나의 VPC에만 연결할 수 있다
동적 라우팅을 사용할 경우  ASN을 지정해줘야함
```

## CGW - Customer Gateway
```
온프레미스 네트워크의 CGD - Customer Gateway Device의 설정 값을 VGW에 제공하기 위한 가상 게이트웨이

특징
CGD에 있는 IP정보 등 CGD와 연결할 수 있는 설정 값들을 CGW에 저장함으로써 VGW는 CGW에 있는 해당 값들을 참조해 CGD에 연결할 수 있게된다
CGW 역시 동적 라우팅을 사용할 경우 ASN을 지정해줘야 함
```

## CGD - Customer Gateway Device
```
Site to Site VPN 연결을 위해 온프레미스에 설치된 라우터 등의 물리적 디바이스 또는 소프트웨어를 의미

특징
CGD가 BGP를 지원하지 않을 경우 VPN 연결시 동적 라우팅을 사용할 수 없다 -> 정적라우팅을 사용해야함
VGW와 CGW의 설정을 모두 구성한 뒤 구성파일을 다운로드 받아 CGD를 설치하게 되면 VPC와 온프레미스 네트워크 간의 VPN Tunnel이 구성됨
```

## VPN Tunnel
```
VPC와 온프레미스 네트워크 간의 서로 데이터를 주고받을 수 있는 암호화된 링크

특징
VPN 터널은 IPSec 프로토콜 스위트를 사용해 데이터를 암호화하여 처리
각 VPN 터널은 인터넷 연결을 위한 Public IP와 내부 연결을 위한 내부 CIDR로 구성
또한 VPN 터널이 생성될 때 AWS에서 기본값을 지정하게 되는데, 암호화, Dead Peer Detection(DPD) 시간 초과, IP 주소 등의 터널옵션을 직접 지정할 수 있다
Site-to-Site VPN을 구성할 때 두 개의 VPN 터널을 생성하게 되는데, 그 이유는 VPN 터널이 사용 불가능 해질 경우 다른 VPN 터널로 자동으로 라우팅되어 고가용성을 겸비할 수 있습니다.
VPN 터널당 최대 대역폭은 1.25Gpbs
```

## 참조
```
https://yoo11052.tistory.com/171
https://limkydev.tistory.com/166
```