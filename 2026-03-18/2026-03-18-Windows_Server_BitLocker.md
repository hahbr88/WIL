# 2026-03-18-Windows_Server_BitLocker

## 개념

![alt text](imgs/image.png)

## 실습 개요

- VMWare 같은 가상머신에는 하드웨어 보안 칩인 TPM이 없다. 따라서 여기서는 그룹 정책을 통해 소프트웨어적으로 우회하는 방법을 사용한다.
- 보호대상이 될 호스트서버(FIRST)와 도난 상황을 시뮬레이션 할 클라이언트PC(WINCLIENT) 총 2대가 필요하다.
- 기본 C드라이브 외에 금고 역할을 할 빈 디스크(약 2GB)를 장착해야한다.

## 서버에 BitLocker 기능 탑재

`서버관리자` 실행 -> `역할 및 기능 추가` -> `기능` -> `BitLocker Drive En암호화` 선택 -> 서버 다시 시작

![alt text](imgs/image-1.png)
![alt text](imgs/image-2.png)

FIRST 가상머신에 하드디스크 추가(여기서는 쉬운 구분을 위해 하드디스크 이름을 BitLockerTest 라고 하였다.)
![alt text](imgs/image-3.png)

디스크 온라인, 초기화를 하자
![alt text](imgs/image-4.png)

온라인과 초기화 완료된 상태
![alt text](imgs/image-5.png)

하드디스크가 잘 인식된것을 확인
![alt text](imgs/image-6.png)

로컬그룹 편집기 실행
![alt text](imgs/image-7.png)

![alt text](imgs/image-8.png)

![alt text](imgs/image-9.png)

![alt text](imgs/image-10.png)

![alt text](imgs/image-11.png)


![alt text](imgs/image-12.png)

![alt text](imgs/image-14.png)


![alt text](imgs/image-15.png)

![alt text](imgs/image-16.png)

![alt text](imgs/image-17.png)

![alt text](imgs/image-18.png)

![alt text](imgs/image-19.png)

여기까지 설정은 안해도 자동으로 들어가지만 규격을 통일하기 위해 해놓는다고 생각하면 된다.

---

`제어판` -> `시스템 밎 보안` -> `BitLocker 드라이브 암호화` (BitLocker 드라이브 암호화 설치 해야 보임)
![alt text](imgs/image-20.png)

새로 만든 BitLockerTest 디스크를 암호화 하자.
![alt text](imgs/image-21.png)

`BitLocker 켜기` 클릭
![alt text](imgs/image-22.png)

`암호를 사용하여 드라이브 잠금해제` 선택 후 암호 입력하고 `다음`
![alt text](imgs/image-23.png)

복구키 백업
![alt text](imgs/image-24.png)

![alt text](imgs/image-26.png)

`암호화 시작` 클릭  
![alt text](imgs/image-27.png)

---

암호화가 완료되면 FIRST를 끄고 다른 가상머신에 BitLockerTest 가상하드를 삽입한다.

그리고 디스크관리에서 디스크를 온라인 하면 아래처럼 락이 걸린 하드디스크를 확인 할 수 있다.
![alt text](imgs/image-28.png)

아까 설정한 암호를 입력하면  
![alt text](imgs/image-29.png)

자물쇠가 회색으로 바뀌며 락이 해제된 것을 확인 할 수 있다.
![alt text](imgs/image-30.png)