# WIL(What I Learned)

## ⚙️ Setting (md 파일 생성)

1. 터미널에 `nano ~/.zshrc` 입력

2. 파일 맨 아래에 아래 내용 추가:

```Bash
# 매번 파일 만들기 귀찮을 때 쓰는 매크로
wil() {
  local filename="$(date +%Y-%m-%d)-$1.md"
  touch "$filename"
  code "$filename"  # VS Code가 설치되어 있다면 바로 열림
}
```
3. Ctrl + O, Enter (저장), Ctrl + X (나가기)

4. 터미널 종료 후 새로 켜거나 `source ~/.zshrc` 입력 (설정 적용)

---

## 2026-02-09

- [컴퓨터구조](/2026-02-09-컴퓨터구조.md)
- [비트(bit)](/2026-02-09-비트(bit).md)
- [BIOS vs UEFI](/2026-02-09-BIOS_vs_UEFI.md)
- [NIC](/2026-02-09-NIC.md)
- [OSI 7 Layer](/2026-02-09-OSI_7Layer.md)
  - [HTTP](/2026-02-09-HTTP.md)
  - [HTTPS](/2026-02-09-HTTPS.md)
