## 컴퓨터 시스템 구조
<img width="326" alt="1" src="https://user-images.githubusercontent.com/93430103/193730392-ba9aaac4-af90-4114-a56e-8c48f4214b10.png">
<img width="297" alt="2" src="https://user-images.githubusercontent.com/93430103/193730978-1a0b5060-d611-40fe-a343-623d3b43ef30.png">


- **CPU :** 매 순간(매 클럭 사이클 마다) 메모리에서 instruction(기계어)를 하나씩 읽어서 실행한다.
- **Memory :** CPU의 작업공간
- 
- **device controller :** 각각의 I/O 디바이스들은 그 디바이스를 전담하는 작은 CPU같은게 붙어있는데 그걸 `device controller`라고 한다.  
  예) 디스크에서 head가 어떻게 움직이고 어떤 data를 읽을지 disk의 내부를 통제하는 것은 CPU의 역할이 아닌 `disk controller`의 역할이다.
