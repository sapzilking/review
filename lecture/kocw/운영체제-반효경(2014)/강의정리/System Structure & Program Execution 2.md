## 컴퓨터 시스템 구조
<img width="954" alt="스크린샷 2022-10-06 오전 12 48 23" src="https://user-images.githubusercontent.com/93430103/194104579-212071aa-b94a-4a78-b5a2-921bf9906897.png">

- **CPU :** 매 순간(매 클럭 사이클 마다) 메모리에서 instruction(기계어, 명령)를 하나씩 읽어서 실행한다.
- **Memory :** CPU의 작업공간
- **device controller :** 각각의 I/O 디바이스들은 그 디바이스를 전담하는 작은 CPU같은게 붙어있는데 그걸 `device controller`라고 한다.  
  예) 디스크에서 head가 어떻게 움직이고 어떤 data를 읽을지 disk의 내부를 통제하는 것은 CPU의 역할이 아닌 `disk controller`의 역할이다.
  - <img width="661" alt="스크린샷 2022-10-06 오전 1 31 43" src="https://user-images.githubusercontent.com/93430103/194113452-9d55428a-26f0-47a6-842a-60f5867fa8f8.png">

  
- CPU의 작업공간인 메인메모리가 있듯이 device controller 들도 그들의 작업공간이 필요하다. 그들의 작업공간이 각각 존재하는데 그걸 `local buffer`라고 부른다.
- **registers :** 메모리보다 더 빠르면서 정보를 저장할 수 있는 작은 공간
- **mode bit :** 지금 CPU에서 실행되는것이 운영체제인지 사용자 프로그램인지 구분해줌.
  - <img width="680" alt="스크린샷 2022-10-06 오전 1 26 05" src="https://user-images.githubusercontent.com/93430103/194112283-0a3f3d50-0f32-4b25-a7cb-b49137947a97.png">

- **timer :** 특정 프로그램이 CPU를 독점하는 것을 막기위한 것.
  - <img width="657" alt="스크린샷 2022-10-06 오전 1 29 37" src="https://user-images.githubusercontent.com/93430103/194113056-8304a13c-0e74-43c6-b246-ce7c891d3866.png">

- 사용자 프로그램은 직접 I/O 장치에 접근할 수 없다. I/O 장치에 접근하는 모든 instruction은 운영체제를 통해서만 가능하다. I/O 작업이 필요하면 스스로 운영체제에게 CPU를 넘겨주고, 그러면 운영체제가 해당하는 작업을 I/O controller에게 시킨 뒤 (이러한 작업은 오래걸리므로) I/O 작업을 요청한 프로그램에게 CPU를 넘기는것이 아닌 다른 프로그램에게 CPU를 넘겨주게 된다. 
  - I/O 작업을 요청한 프로그램은 언제까지 기다리느냐?
    1. I/O controller가 요청받은 작업이 끝난 뒤 CPU에게 interrupt를 건다.
    2. cpu가 다음 instruction을 실행하기 전 interrupt가 있으면 CPU의 제어권을 운영체제로 넘긴다.
    3. 운영체제가 interrupt에 대해 분석 후 disc controller의 local buffer에 있는 값을 I/O를 요청한 프로그램의 메모리에 copy한 뒤 interrupt발생으로 제어권을 뺏긴 프로그램에게 다시 제어권을 준다.
    4. 그 뒤 차례가 오면 다시 I/O 요청을 한 사용자 프로그램이 실행된다.

- **memory controller** : CPU와 DMA controller가 같은 메모리에 접근하지 않도록 해주는 역할.
- **DMA controller** : CPU가 잦은 interrupt를 받아 disc controller의 local buffer의 데이터를 copy하는 작업이 너무 오버헤드가 크고 효율이 떨어지기 때문에, DMA controller를 둠으로써 local buffer의 데이터를 메인 메모리에 copy하는 작업을 다 마친뒤 CPU에게 interrupt를 한번만 걸어서 CPU를 좀더 효율적으로 사용할 수 있도록 해준다.

<img width="1099" alt="스크린샷 2022-10-06 오전 1 47 48" src="https://user-images.githubusercontent.com/93430103/194116535-b3b79e2d-47ab-49fe-ac07-b8c962a96d74.png">

<img width="1063" alt="스크린샷 2022-10-06 오전 1 54 23" src="https://user-images.githubusercontent.com/93430103/194117694-2e574fc4-d477-4883-93e0-012f67f42870.png">

<img width="657" alt="스크린샷 2022-10-06 오전 1 59 55" src="https://user-images.githubusercontent.com/93430103/194118686-dae2d350-07dd-4252-8e8e-dd1893e09896.png">

<img width="692" alt="스크린샷 2022-10-06 오전 2 01 50" src="https://user-images.githubusercontent.com/93430103/194119070-41766d36-2dfe-4d1c-9bbe-c3250a6f9650.png">








  
