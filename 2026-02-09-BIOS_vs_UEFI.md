# 2026-02-09-BIOS_vs_UEFI

## 1️⃣ BIOS (Basic Input Output System)

- BIOS란?
  - 전통적인 PC 펌웨어
  - CPU가 ROM에서 실행하는 최초 프로그램

- BIOS 부팅 과정
  1. 전원 ON
  2. POST 수행 (CPU, RAM, 키보드 등 검사)
  3. 부팅 장치 탐색 (MBR)
  4. 부트로더 실행
  5. OS 로딩

- BIOS 특징
  - 16비트 모드
  - MBR 방식 사용
  - 디스크 최대 2TB 제한
  - 키보드 중심 인터페이스
  - 느림

## 2️⃣ UEFI (Unified Extensible Firmware Interface)

- UEFI란?  
BIOS를 대체하는 차세대 펌웨어 표준

- UEFI 부팅 과정
  1. 전원 ON
  2. 하드웨어 초기화
  3. EFI System Partition(ESP) 접근
  4. EFI 부트로더 실행
  5. OS 로딩  

- UEFI 특징
- 32/64비트
- GPT 방식 사용
- 디스크 크기 9.4ZB까지 지원
- 마우스 지원 GUI
- 네트워크 부팅 가능
- 빠른 부팅

## 3️⃣ BIOS vs UEFI 핵심 비교 (시험 단골)

|구분|BIOS|UEFI|
|------|---|---|
|저장 위치|ROM|Flash ROM|
|CPU 모드|16비트|32/64비트|
|파티션|MBR|GPT|
|디스크 제한|2TB|거의 무제한|
|인터페이스|텍스트|GUI|
|보안|취약|Secure Boot|

 Secure Boot란?
UEFI 기능
서명된 OS만 부팅 허용
루트킷, 부트킷 방어