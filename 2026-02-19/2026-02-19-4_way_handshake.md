# 2026-02-19-

## 🔴 3️⃣ 4-Way Handshake (연결 종료 과정)

> 목적: “통신 끝내자”

연결 종료는 **양방향이 따로 종료**되기 때문에 4단계가 필요.

---

### 📌 기본 흐름

```bash
Client                   Server
  | --------- FIN --------> |
  | <-------- ACK --------- |
  | <-------- FIN --------- |
  | --------- ACK --------> |
```

---

### 🔍 단계별 자세히

#### ① FIN (Client → Server)

```bash
FIN
Seq = X
```

👉 "나 이제 보낼 데이터 없어"

Client → FIN_WAIT_1

---

#### ② ACK (Server → Client)

```bash
ACK = X+1
```

👉 "알겠어"

Server → CLOSE_WAIT
Client → FIN_WAIT_2

⚠ 여기서 서버는 아직 데이터 보낼 수 있음

---

#### ③ FIN (Server → Client)

```bash
FIN
Seq = Y
```

👉 "나도 이제 끝"

Server → LAST_ACK

---

#### ④ ACK (Client → Server)

```bash
ACK = Y+1
```

👉 "확인 완료"

Client → TIME_WAIT (2MSL 대기)

---

### 🕒 왜 TIME_WAIT이 필요한가?

* 마지막 ACK가 손실될 수 있음
* 혹시 모를 지연 패킷 정리

👉 안전하게 정리하기 위한 대기 시간

---

### 🔥 왜 종료는 4번일까?

연결은 동시에 시작 가능하지만
종료는 **각자 데이터 종료 시점이 다를 수 있기 때문**

👉 그래서 FIN이 두 번 필요
