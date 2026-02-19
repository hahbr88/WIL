# 2026-02-19-Create_User

## create user

### 1. PowerShell을 이용한 일괄 생성 (가장 추천!)

여러 명의 사용자를 만들 때는 명령어를 사용하는 것이 가장 빠릅니다.

1. **시작** 버튼 우클릭 후 **Windows PowerShell (관리자)**를 실행.
2. 아래 코드를 복사해서 붙여넣고 Enter를 누르세요. (비밀번호는 예시로 `Pa$$w0rd`를 설정함. 필요하다면 변경할것)

```powershell
# 생성할 사용자 목록
$users = "user1", "user2", "user3"
$password = ConvertTo-SecureString "Pa$$w0rd123!" -AsPlainText -Force

foreach ($u in $users) {
    New-LocalUser -Name $u -Password $password -FullName "$u Account" -Description "Created for testing"
    Write-Host "사용자 $u 생성 완료!" -ForegroundColor Green
}

```

> **참고:** Windows 보안 정책상 비밀번호는 숫자, 영문 대소문자, 특수문자가 섞인 복잡한 형태여야 생성됩니다.

---

### 2. 컴퓨터 관리(GUI)를 이용한 방법

윈도우 창에서도 직접 클릭하여 생성할 수 있음.

1. **시작** 버튼 우클릭 -> **컴퓨터 관리(Computer Management)**를 선택합니다.
2. 왼쪽 메뉴에서 **로컬 사용자 및 그룹** -> **사용자** 폴더를 클릭합니다.
3. 중앙 빈 공간에 마우스 우클릭 후 **새 사용자(New User)**를 선택합니다.
4. **사용자 이름**에 `user1`을 입력하고, 사용할 비밀번호를 입력합니다.  
  `다음 로그온 시 사용자가 반드시 암호를 변경해야 함` 되도록이면 체크를 하는게 좋다  
  (ex: 신규직원이 와서 새로 계정을 만들어주는 경우 그 직원만 비밀번호를 알게 됨).
5. **만들기**를 누르고 `user2`, `user3`에 대해서도 반복.

---

### 3. 명령 프롬프트(CMD)를 이용한 방법

전통적인 `net user` 명령어를 사용하는 방법.

1. **시작** -> `cmd` 검색 -> **관리자 권한으로 실행**.
2. 아래 명령어를 한 줄씩 입력.

```cmd
net user user1 Pa$$w0rd123! /add
net user user2 Pa$$w0rd123! /add
net user user3 Pa$$w0rd123! /add

```

---

### 💡 팁: 생성된 유저 확인하기

계정이 잘 만들어졌는지 확인하려면 PowerShell에서 다음 명령어를 입력.
`Get-LocalUser | Where-Object {$_.Name -like "user*"}`
