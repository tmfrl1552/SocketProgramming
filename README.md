# SocketProgramming
## TCP와 UDP를 비교해보는 간단한 소켓프로그래밍입니다.

### UDPClient.py   
```python
from socket import  *
from pip._vendor.distlib.compat import raw_input

serverName = 'localhost'
serverPort = 12000
clientSocket = socket(AF_INET, SOCK_DGRAM)
message = raw_input('Input lowercase sentence:')
clientSocket.sendto(message.encode(),(serverName, serverPort))
modifiedMessage, serverAddress = clientSocket.recvfrom(2048)
print('From Server: '+modifiedMessage.decode())
clientSocket.close()
```
     
        
      
* ```pythonfrom socket import  *   ```   
socket 모듈은 파이썬에서 모든 네트워크 통신의 기본을 구성한다. 이 라인을 포함함으로써 프로그램 내에 소켓을 생성할 수 있다.

* ```python from pip._vendor.distlib.compat import raw_input   ```   
파이썬 내장 함수인 raw_input을 사용하기 위한 모듈을 선언한 것이다.

* ```python serverName = 'localhost'   ```   
serverPort = 12000   
첫 번째 라인은 문자열 ‘localhost’를 변수 serverName에 할당한다. 호스트 이름을 사용하는 경우 IP 주소를 얻기 위해 DNS 검색에 자동으로 수행된다. 두 번째 라인은 정수 변수 serverPort에 12000을 할당한다.

* ```python clientSocket = socket(AF_INET, SOCK_DGRAM)   ```   
clientSocket이라는 클라이언트 소켓을 생성한다. 첫 번째 파라미터는 주소군(family)을 나타낸다. 특히, AF_INET은 하부 네트워크가 IPv4를 사용하고 있음을 나타낸다. 두 번째 파라미터는 소켓이 UDP 소켓임을 의미하는 SOCK_DGRAM 타입을 나타낸다. 소켓을 생성할 때 클라이언트 소켓의 포트 번호를 명시하지 않아도 되며, 운영체제가 이 작업을 대신 수행한다. 이제 클라이언트 프로세스의 문이 생성되어 있으며 이 문을 통해 보낼 메세지를 생성하게 된다.

* ```python message = raw_input('Input lowercase sentence:') ```     
이 명령이 수행되면, 클라이언트 쪽의 사용자에게 “Input lowercase sentence:”의 프롬프트가 나타난다. 그러면 사용자는 키보드를 이용하여 변수 message에 할당되는 라인을 입력한다. 이제 소켓과 메시지를 생성했으며 이 메시지를 소켓을 통해 목적지 호스트로 보내게 된다.

* ```python clientSocket.sendto(message.encode(),(serverName, serverPort))  ```    
소켓으로 바이트 형태로 보내기 위해 먼저 encode() 메소드를 사용해서 문자열 타입의 메시지를 바이트 타입으로 변환한다. sendto() 메소드는 목적지 주소(serverName, sereverPort)를 메시지에 붙이고 그 패킷을 프로세스의 소켓인 clientSocket으로 보낸다.

* ```python modifiedMessage, serverAddress = clientSocket.recvfrom(2048)  ```    
패킷이 클라이언트의 소켓에 인터넷으로부터 도착하면 패킷 데이터는 변수 modifiedMessage에 할당되고 패킷의 소스 주소는 변수 serverAddress에 할당된다. 변수 serverAddresss는 서버의 IP 주소와 서버의 포트 번호를 포함한다. UDPClient 프로그램은 처음부터 서버 주소를 알고 있기 때문에 실제로 이 서버 주소 정보를 필요로 하지 않는다. recfrom 메소드 또한 2048을 버퍼 크기로 받아들인다.

* ```python print('From Server: '+modifiedMessage.decode())   ```   
메시지를 바이트에서 문자열로 변환한 다음에 사용자의 화면에 modifiedMessage를 출력한다. 사용자가 입력한 원래의 라인이 대문자로 전환돼서 출력한다.

* ```python clientSocket.close()   ```   
소켓을 닫으며, 그 후 프로세스가 종료된다.
   
       
*** 

### UDPServer.py
```python
from socket import *
serverPort = 12000
serverSocket = socket(AF_INET, SOCK_DGRAM)
serverSocket.bind(('', serverPort))
print('The server is ready to receive')
while True:
    message, clientAddress = serverSocket.recvfrom(2048)
    print(message.decode())
    modifiedMessage = message.decode().upper()
    serverSocket.sendto(modifiedMessage.encode(), clientAddress)
```

UDPServer의 시작은 UDPClient와 유사하다. 즉, 소켓 모듈을 사용하고(import), 정수 변수 serverPort에 12000을 할당하며, SOCK_DGRAM 타입의 소켓을 생성한다. UDPClient와 상이한 코드의 첫 번째 라인은 다음과 같다.

* ```python serverSocket.bind(('', serverPort))```   
포트 번호 12000을 서버의 소켓에 할당한다. 즉, UDPClient의 코드는 명시적으로 포트 번호를 소켓에 할당한다. 이렇게 서버 IP 주소의 12000 포트로 패킷을 보내면 해당 소켓으로 패킷이 전달된다. 그 뒤 UDPServer는 클라이언트로부터 계속해서 패킷을 수신하고 처리할 수 있도록 while 루프로 들어간다. While 루프에서 UDPServer는 패킷이 도착하기를 기다린다.

* ```python message, clientAddress = serverSocket.recvfrom(2048)```   
패킷이 서버의 소켓에 도착하면, 패킷의 데이터는 변수 message에 할당되고 패킷의 소스 주소는 변수 clientAddress에 할당된다. 변수 clientAddress는 클라이언트의 IP 주소와 클라이언트의 포트 번홀ㄹ 포함한다. 여기서, UDPServer는 일반 우편에서의 반송 주소와 마찬가지로 반송 주소를 제공해야 할 때 이 주소 정보를 사용한다. 소스 주소 정보를 통해 서버는 응답을 어디로 보내야 할 지 알게 된다.

* ```python modifiedMessage = message.decode().upper() ```   
간단한 애플리케이션의 주요 부분으로 클라이언트로부터의 라인을 받아서 메소드 upper()을 이용하여 대문자로 변환한다.

* ```python serverSocket.sendto(modifiedMessage.encode(), clientAddress) ```   
클라이언트의 주소를 대문자로 변환된 메시지에 붙이고, 그 결과로 만들어진 패킷을 서버의 소켓으로 보낸다. 그 뒤 인터넷은 패킷을 클라이언트 주소로 전달한다. 서버는 패킷을 보낸 후 while 루프에 머무르며 다른 UDP 패킷이 도착하기를 기다린다.



***



### TCPClient.py
```python
from socket import *
from pip._vendor.distlib.compat import raw_input

serverName = 'localhost'
serverPort = 12000
clientSocket = socket(AF_INET, SOCK_STREAM)
clientSocket.connect((serverName, serverPort))
sentence = raw_input('Input lowercase sentence:')
clientSocket.send(sentence.encode())
modifiedSentence = clientSocket.recv(2048)
print('From Server:', modifiedSentence.decode())
clientSocket.close()
```


UDP 구현과 차이가 많이 나는 코드들을 살펴보면,

* ```python clientSocket = socket(AF_INET, SOCK_STREAM) ```   
clientSocket이라고 하는 클라이언트 소켓을 생성한다. 첫 번째 파라미터는 하위 네트워크가 IPv4를 사용함을 의미한다. 두 번째 파라미터는 소켓 SOCK_STREAM 타입임(TCP 소켓을 의미)을 나타낸다. 클라이언트 소켓을 생성할 때 해당 소켓의 포트 번호를 명시하지 않는다. 대신 운영체제가 하도록 내버려둔다.

* ```python clientSocket.connect((serverName, serverPort)) ```   
클라이언트가 TCP 소켓을 이용하여 서버로 데이터를 보내기 전에 TCP 연결이 먼저 클라이언트와 서버 사이에 설정되어야 한다. 우의 라인은 클라이언트와 서버 간에 TCP 연결을 시작한다. Connect() 메소드의 파라미터는 연결의 서버 쪽 주소이다. 이 라인이 수행된 후에 3-way handshake가 수행되고 클라이언트와 서버 간에 TCP 연결이 설정된다.

* ```python sentence = raw_input('Input lowercase sentence:') ```   
사용자로부터 문장을 획득한다. 문자열 sentence는 사용자가 리턴 키를 입력하여 문장을 마칠 때까지 문자를 계속해서 모은다.

* ```python clientSocket.send(sentence.encode()) ```   
문자열 sentence를 클라이언트 소켓을 통해 TCP 연결로 보낸다. UDP 소켓처럼 프로그램이 패킷을 명시적으로 생성하지 않으며 패킷에 목적지 주소를 붙이지 않는다. 대신에 클라이언트 프로그램은 단순히 문자열 sentence에 있는 바이트를 TCP Connection에게 제공한다. 그리고 클라이언트는 서버로부터 바이트를 수신하기를 기다린다.

* ```python modifiedSentence = clientSocket.recv(2048) ```   
서버로부터 온 문자를 문자열 modifiedSentence에 모은다. 라인이 리턴 키로 끝날 때까지 modifiedSentence에 문자가 계속해서 쌓인다. 대문자로 변환된 문장을 출력한 후 클라이언트 소켓을 닫는다.

* ```python clientSocket.close() ```   
소켓을 닫고 클라이언트와 서버 간의 TCP 연결을 닫는다. 이는 클라이언트의 TCP가 서버의 TCP에게 TCP 메시지를 보내도록 한다.








***




### TCPServer.py
```python 
from socket import *
serverPort = 12000
serverSocket = socket(AF_INET, SOCK_STREAM)
serverSocket.bind(('', serverPort))
serverSocket.listen(1)
print('The server is ready to receive')
while True:
    connectionSocket, addr = serverSocket.accept()
    sentence = connectionSocket.recv(2048).decode()
    print(sentence)
    capitalizedSentence = sentence.upper()
    connectionSocket.send(capitalizedSentence.encode())
    connectionSocket.close()
```


* ```python serverSocket = socket(AF_INET, SOCK_STREAM)```   
TCPClient와 마찬가지로, 서버는 TCP 소켓을 만든다. 

* ```python serverSocket.bind(('', serverPort))```   
UDPServer 처럼 서버 포트 번호 serverPort를 소켓과 연관시킨다.

* ```python serverSocket.listen(1)```   
그러나 TCP에서는 serverSocket이 대기하는 소켓이 된다. 출입문을 설정한 후에 임의의 클라이언트가 이 문을 두드리기를 기다린다. 이 라인은 서버가 클라이언트로부터의 TCP 연결 요청을 듣도록 한다. 파라미터는 큐되는 연결의 최대 수를 나타낸다.(적어도 1)

* ```python connectionSocket, addr = serverSocket.accept()```   
클라이언트가 이 문을 두드리면 프로그램은 serverSocket을 위한 accept() 메소드를 시작해서 이 클라이언트에게 지정된 connectionSocket이라는 새로운 소켓을 서버에 생성한다. 그 뒤 클라이언트와 서버는 핸드셰이킹을 완료해서 클라이언트의 clientSocket과 서버의 connectionSocket 간에 TCP 연결을 생성한다. TCP 연결이 설정되었으므로 클라이언트와 서버는 이제 이 연결을 통해 서로에게 바이트를 보낼 수 있다. TCP의 경우 한쪽에서 보낸 모든 바이트가 다른 쪽에 도착하는 것을 보장할 뿐만 아니라 순서대로 도착하는 것을 보장한다.

* ```python connectionSocket.close()```   
클라이언트에게 수정된 문장을 보낸 후 연결 소켓을 닫는다. 그러나 serverSocket이 열려 있기 때문에 다른 클라이언트가 출입문을 두드릴 수 있고 서버에게 수정할 문장을 보낼 수 있다.
