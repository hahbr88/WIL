# ⚙️ Setting (`wil` 문서 생성 자동화)

`wil 문서제목` 명령으로 아래 작업을 한 번에 처리하는 세팅이다.

1. 오늘 날짜 기준 폴더 생성
2. 해당 폴더 안에 마크다운 문서 생성
3. 생성한 문서 링크를 `README.md`에 같은 형식으로 자동 추가

예시:

```powershell
wil DNS
```

실행 결과:

- `2026-03-24/2026-03-24-DNS.md` 생성
- `README.md`에 `## 2026-03-24` 섹션이 있으면 해당 섹션의 마지막에 링크 추가
- 해당 날짜 섹션이 없으면 `---` 구분선 뒤에 새 섹션을 만들고 링크 추가

---

## Mac

1. 터미널에서 `nano ~/.zshrc` 실행
2. 파일 맨 아래에 아래 함수를 추가

```bash
wil() {
  local topic="$1"

  if [ -z "$topic" ]; then
    echo "Usage: wil <문서제목>"
    return 1
  fi

  local date_str
  date_str=$(date +%Y-%m-%d)

  local dir_name="$date_str"
  local file_name="$date_str-$topic.md"
  local file_path="$dir_name/$file_name"
  local readme_path="README.md"
  local readme_link="- [$topic](/$file_path)"

  mkdir -p "$dir_name"

  if [ ! -f "$file_path" ]; then
    echo "# $date_str-$topic" > "$file_path"
    echo "Created: $file_path"
  else
    echo "File already exists: $file_path"
  fi

  if [ -f "$readme_path" ]; then
    if ! grep -Fq "$readme_link" "$readme_path"; then
      if grep -Fq "## $date_str" "$readme_path"; then
        awk -v heading="## $date_str" -v newline="$readme_link" '
          $0 == heading { in_section=1 }
          in_section && /^## / && $0 != heading && !done {
            print newline
            done=1
            in_section=0
          }
          { print }
          END {
            if (in_section && !done) {
              print newline
            }
          }
        ' "$readme_path" > "$readme_path.tmp" && mv "$readme_path.tmp" "$readme_path"
      else
        {
          printf "\n---\n\n## %s\n\n%s\n" "$date_str" "$readme_link"
        } >> "$readme_path"
      fi
      echo "Updated: $readme_path"
    else
      echo "README already contains: $readme_link"
    fi
  else
    echo "# WIL(What I Learned)" > "$readme_path"
    printf "\n---\n\n## %s\n\n%s\n" "$date_str" "$readme_link" >> "$readme_path"
    echo "Created: $readme_path"
  fi

  # code "$file_path"
}
```

3. 저장 후 `source ~/.zshrc` 실행

---

## Windows

Windows에서는 `.zshrc` 대신 PowerShell 프로필 파일에 함수를 등록하면 된다.

### 1. 프로필 파일 만들기

```powershell
if (!(Test-Path $PROFILE)) { New-Item -Type File -Path $PROFILE -Force }
```

### 2. 프로필 열기

```powershell
notepad $PROFILE
```

### 3. 아래 함수 추가

```powershell
function wil($topic) {
  if ([string]::IsNullOrWhiteSpace($topic)) {
    Write-Host "Usage: wil <문서제목>" -ForegroundColor Yellow
    return
  }

  $dateStr = Get-Date -Format "yyyy-MM-dd"
  $dirPath = Join-Path -Path (Get-Location) -ChildPath $dateStr
  $fileName = "$dateStr-$topic.md"
  $filePath = Join-Path -Path $dirPath -ChildPath $fileName
  $readmePath = Join-Path -Path (Get-Location) -ChildPath "README.md"
  $readmeLink = "- [$topic](/$dateStr/$fileName)"

  if (!(Test-Path $dirPath)) {
    New-Item -ItemType Directory -Path $dirPath | Out-Null
  }

  if (!(Test-Path $filePath)) {
    "# $dateStr-$topic" | Out-File -FilePath $filePath -Encoding utf8
    Write-Host "Created: $filePath" -ForegroundColor Green
  } else {
    Write-Host "File already exists: $filePath" -ForegroundColor Yellow
  }

  if (!(Test-Path $readmePath)) {
    @"
# WIL(What I Learned)

## $dateStr

$readmeLink
"@ | Out-File -FilePath $readmePath -Encoding utf8

    Write-Host "Created: $readmePath" -ForegroundColor Green
    return
  }

  $readmeContent = Get-Content -Path $readmePath -Raw -Encoding utf8

  if ($readmeContent -match [regex]::Escape($readmeLink)) {
    Write-Host "README already contains: $readmeLink" -ForegroundColor Yellow
    return
  }

  $heading = "## $dateStr"
  $newSection = @"

---

## $dateStr

$readmeLink
"@

  if ($readmeContent -match [regex]::Escape($heading)) {
    $lines = [System.Collections.Generic.List[string]]::new()
    $lines.AddRange([string[]](Get-Content -Path $readmePath -Encoding utf8))

    $headingIndex = $lines.IndexOf($heading)
    $insertIndex = $lines.Count

    for ($i = $headingIndex + 1; $i -lt $lines.Count; $i++) {
      if ($lines[$i] -match '^## ') {
        $insertIndex = $i
        break
      }
    }

    while ($insertIndex -gt 0 -and [string]::IsNullOrWhiteSpace($lines[$insertIndex - 1])) {
      $insertIndex--
    }

    $lines.Insert($insertIndex, $readmeLink)
    $updatedContent = ($lines -join "`r`n").TrimEnd() + "`r`n"
  } else {
    $trimmed = $readmeContent.TrimEnd()
    $updatedContent = $trimmed + "`r`n" + $newSection + "`r`n"
  }

  $updatedContent | Out-File -FilePath $readmePath -Encoding utf8
  Write-Host "Updated: $readmePath" -ForegroundColor Green

  # code $filePath
}
```

### 4. 저장 후 적용

```powershell
. $PROFILE
```

점(`.`) 하나를 쓰고 한 칸 띄운 뒤 `$PROFILE`을 입력하면 된다.

---

## 실행 권한 오류가 날 때

PowerShell에서 프로필 로드가 막히면 아래 명령을 한 번 실행한다.

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

이후 `Y`를 입력해 승인하면 된다.

---

## 동작 방식

`wil DNS`를 실행하면 다음 순서로 처리된다.

1. 오늘 날짜 폴더가 없으면 생성
2. `날짜-문서제목.md` 파일 생성
3. 문서 첫 줄에 `# 날짜-문서제목` 작성
4. `README.md`에 `- [문서제목](/날짜/파일명.md)` 형식으로 링크 추가
5. 같은 날짜 섹션이 이미 있으면 맨 아래에 이어서 추가
6. 이미 같은 링크가 있으면 중복 추가하지 않음
