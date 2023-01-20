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
   ### WARNING
   `make on`을 했을 때 백그라운드로 실행되게 해보려고 했지만 안돼서 결국 하나의 터미널은 점유하게 되니 '참고'하시면 됩니다.   
   

도커 이미지가 빌드되고, 컨테이너도 잘 실행되면 본격적으로 패킷 분석을 시작해 본다.
1. 컨테이너 실행
   패킷 분석을 하기 위해서는 컨테이너 내부에서 실행해야 되는데, 해당 명령어는 다음과 같다.
   `docker exec -it irc bash`   


2. 서버 실행
   `inspircd` 서버의 경우 `setup.sh` bash 파일로 돌려놓았기 때문에 별도의 명령어 없이 운영되고 있을 것이다.   


3. irssi 클라이언트로 서버 접속
   `irssi` 클라이언트로 서버에 접속하는 방법은 다음의 명령어를 터미널에 입력하면 된다.
   `irssi -c <ip address> -p <port> -n <nickname>`
   -c : IP주소나 도메인을 입력
   -p : 포트 번호를 입력
   -n : 닉네임 입력

   우리는 로컬환경에서 실행할 것이고, 기본 inspircd 서버는 6667이므로
   `irssi -c localhost -p 6667 -n test`
   로 접석하면 된다.   


4. tcpflow로 메세지 확인
   또 다른 터미널로 컨테이너에 접속해서
   `tcpflow -i lo port 6667 -c`
   를 입력하면 6667 포트로 지나가는 패킷의 데이터를 볼 수 있다.
   -i : 인터페이스 지정 (IP주소, 도메인 등. lo : loopback - localhost)
   port : 지정한 포트 번호
   -c : 지나간 패킷의 정보를 console을 통해 stdout 해주는 옵션

   
   출력 메세지 구조는 다음과 같다.
   {sender}-{receiver}: {message}   


5. inspircd 서버를 debug 옵션 주기
   setup폴더의 bash 파일을 보면
   `inspircd --runasroot --nofork`
   로 실행을 하고 있는데 해당 명령어 뒤에 `--debug`를 추가하면 서버로 들어오는 패킷을 바로 볼 수 있다.
   `inspircd --runasroot --nofork --debug`
   이건 해도 되고 안 해도 되고 취향에 맞게 설정하면 된다.   


### Reference
https://80000coding.oopy.io/1ac75b59-6930-4297-9c9d-7dec31eff19d
