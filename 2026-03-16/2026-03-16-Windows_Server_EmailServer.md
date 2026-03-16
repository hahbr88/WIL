# 2026-03-16-Windows_Server_EmailServer

## 개념

E-mail 송수신에서 사용되는 기본적인 프로토콜

- **STMP(Simple Mail Transpfer Protocol)**  
  클라이언트가 메일을 보내거나 E-Mail 서버끼리 메일을 주고받을때 사용

- **POP3(Post Office Protocol)**  
E-Mail 서버에 도착한 메일을 클라이언트로 가져올때 사용

- **IMAP(Internet Mail Access Protocol)**  
POP3와 동일하게 E-Mail 서버에 도착한 메일을 클라언트로 가져올때 사용

## 실습 개요

VMware를 사용하여 E-mail 서버 2대 설치 -> 다른 도메인을 가진 2개의 E-mail 서버
VMware는 원래 사설네트워크지만 여기서는 외부망이라 가정
모든 컴퓨터의 DNS서버는 FIRST(192.168.111.10)으로 통일

|vm|ip|role|
|---|---|-------|
|FIRST|192.168.111.10|DNS서버, first.com 메일서버|
|SECOND|192.168.111.20|second.com 메일서버|
|THIRD|192.168.111.30|메일클라이언트|
|WINCLIENT|192.168.111.000|메일클라이언트|
