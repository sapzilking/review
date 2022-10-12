## 동기식 입출력과 비동기식 입출력

<img width="679" alt="Screenshot 2022-10-13 at 1 28 58 AM" src="https://user-images.githubusercontent.com/93430103/195398186-017cdac4-a577-4830-8f87-767bce15f199.png">

<img width="702" alt="Screenshot 2022-10-13 at 1 47 27 AM" src="https://user-images.githubusercontent.com/93430103/195401649-1870a2e2-4860-437a-bd33-4a4e2ab58012.png">

- 인터럽트가 발생하면 CPU는 자신의 제어권을 OS에 넘겨주고 실행 중이던 프로세스를 멈추고 인터럽트를 처리하는 등의 오버헤드가 발생한다.  
  이렇게 잦은 인터럽트는 CPU를 효율적으로 사용할 수 없게 한다.  
  메모리를 접근할 수 있는 장치는 CPU뿐이지만 DMA라는 장치도 접근할 수 있게 하여, 인터럽트를 DMA가 처리하도록 하고 어느정도 쌓이면 CPU로 인터럽트를 발생시켜 CPU로의 인터럽트 빈도를 줄여 CPU의 사용효율을 높인다.

<img width="661" alt="Screenshot 2022-10-13 at 1 56 11 AM" src="https://user-images.githubusercontent.com/93430103/195403453-4d4530ab-522e-4ab1-bfae-aea7c8fcec90.png">

<img width="707" alt="Screenshot 2022-10-13 at 1 57 40 AM" src="https://user-images.githubusercontent.com/93430103/195403654-02867d13-66bf-4e43-ba24-ac3e3796c9ad.png">

- **저장장치 계층 구조** : 위로 갈수록 빠르고 가격이 비싸다.  
  일반적으로 위의 3개(Registers, Cache Memory, Main Memory)는 휘발성이고 아래 3개(Magnetic Disk, Optical Disk, Magnetic Tape)는 비휘발성이다.  
  CPU에서 직접 접근할 수 있는 메모리 스토리지 매체를 Primary라고 하고 CPU가 직접 접근해서 처리 못하는 것을 Secondary라고 부른다.
- **캐싱의 목적** : 재사용


