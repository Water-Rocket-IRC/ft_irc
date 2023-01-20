# packet_tester

## About Packet Tester
IRC는 Internet Relay Chat 프로토콜인데 RFC 공식문서를 지키는 착한 클라이언트는 존재하지 않는다.
따라서 과제를 하기 위해 클라이언트가 사용하는 패킷을 분석하고 대응할 수 있게 만들어야 한다.
docker를 쓰면 각각의 운영체제 버전 상관 없이 테스트를 할 수 있어 ubuntu 환경의 도커파일을 만들어 두었다.

## How to use
사용하는 방법은 간단하다
1. git clone 받기
2. `cd packet_tester`로 디렉토리 이동하기
3. `make`

make 규칙은 다음과 같이 정의해 놓았다.
- `make`: 도커파일로 도커 이미지를 빌드한다. (도커 컨테이너를 실행하지는 않음!)
- `make on` : 빌드된 도커 이미지르 실행한다.
   (`make` 이후에 `on`을 할 것 -> 귀찮으면 on의 dependancy로 추가해도 무방하지만 컨테이너 실행할 때 마다 이미지를 빌드하는 게 더 귀찮다고 생각했음)
- `make down` : 실행중인 컨테이너를 멈춘다.
- `make clean` : 실행중인 컨테이너르 멈추고 컨테이너를 삭제한다.
- `make fclean` : 도커 이미지를 삭제한다. (`make clean`하고 `make fclean`할 것! dependancy로 clean을 두었는데 안 됨...)
