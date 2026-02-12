# ⚙️ Setting (md 파일 생성)

## MAC

1. 터미널에 `nano ~/.zshrc` 입력

2. 파일 맨 아래에 아래 내용 추가:

    ```bash
        wil() {
      # 1. 폴더명 (예: 2026-02)
      local dir_name=$(date +%Y-%m-%d)
      
      # 2. 파일명 (예: 2026-02-09-주제.md)
      local file_name=$(date +%Y-%m-%d)-$1.md
      
      # 3. 폴더가 없으면 생성 (-p 옵션으로 에러 방지)
      mkdir -p "$dir_name"
      
      # 4. 해당 폴더 안에 파일 생성
      touch "$dir_name/$file_name"
      
      echo "✅ Created: $dir_name/$file_name"
      
      # (선택사항) 바로 VS Code로 열고 싶다면 아래 주석 제거
      # code "$dir_name/$file_name"
    }
    ```

3. Ctrl + O, Enter (저장), Ctrl + X (나가기)

4. 터미널 종료 후 새로 켜거나 `source ~/.zshrc` 입력 (설정 적용)

---

## Windows

`nano`나 `.zshrc`는 리눅스나 맥 환경의 도구들이라 윈도우 기본 상태에서는 작동하지 않는다. 윈도우(PowerShell) 환경에 맞는 방법은 아래와 같다.

윈도우에서는 `.zshrc` 대신 **PowerShell 프로필** 파일에 함수를 등록해야 합니다.

### 1. 프로필 파일 열기

- 프로필 폴더와 파일을 강제로 생성하기 이 명령은 폴더가 없으면 만들고, 빈 파일을 생성합니다. (이미 있으면 무시되니 안심하세요.)

```PowerShell
if (!(Test-Path $PROFILE)) { New-Item -Type File -Path $PROFILE -Force }
```

- PowerShell 창에 아래 명령어를 입력해 보세요. (메모장으로 설정 파일을 여는 명령입니다.)

```powershell
notepad $PROFILE
```

### 2. 코드 붙여넣기

열린 메모장 맨 아래에 아래 코드를 복사해서 붙여넣으세요. 윈도우 문법에 맞게 수정한 코드입니다.

```powershell
function wil($topic) {
    # 1. 날짜 변수 설정
    $dateStr = Get-Date -Format "yyyy-MM-dd"
    
    # 2. 폴더 생성 (없으면 만들고, 있으면 넘어감)
    if (!(Test-Path $dateStr)) {
        New-Item -ItemType Directory -Path $dateStr | Out-Null
    }
    
    # 3. 파일명 및 경로 설정
    $fileName = "$dateStr-$topic.md"
    $filePath = Join-Path -Path $dateStr -ChildPath $fileName
    
    # 4. 파일 생성 (이미 있으면 덮어쓰지 않음)
    if (!(Test-Path $filePath)) {
        New-Item -ItemType File -Path $filePath | Out-Null
        Write-Host "✅ Created: $filePath" -ForegroundColor Green
    } else {
        Write-Host "ℹ️ File already exists: $filePath" -ForegroundColor Yellow
    }

    # 5. VS Code로 바로 열기 (설치되어 있다면)
    # code $filePath
}

```

### 3. 저장 및 적용

1. 메모장을 **저장(Ctrl + S)**하고 닫습니다.
2. PowerShell 창으로 돌아와 설정을 적용합니다.

```powershell
. $PROFILE

```

*(점 찍고 한 칸 띄우고 `$PROFILE`입니다)*

---

### ⚠️ 혹시 "권한 오류"가 난다면?

명령어를 입력했는데 "스크립트를 실행할 수 없어서..."라는 빨간 글씨가 나온다면, 보안 설정을 한 번만 풀어줘야 합니다. 터미널을 **관리자 권한**으로 열고 아래를 입력하세요.

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser

```

그다음 `Y`를 누르시면 됩니다.

---

### 🚀 이제 테스트해 보세요

이제 윈도우에서도 똑같이 사용할 수 있습니다.

```powershell
wil DNS

```

이렇게 치면 `2026-02-12` 폴더가 생기고 그 안에 `2026-02-12-DNS.md` 파일이 생성됨.

---