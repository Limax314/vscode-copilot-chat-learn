# WSL 설치 오류(HCS_E_SERVICE_NOT_AVAILABLE) 해결 방법

본 가이드는 `wsl --install` 명령어 실행 중 발생하는
`HCS_E_SERVICE_NOT_AVAILABLE` 오류에 대한 원인 분석 및
해결 방법을 설명합니다.

> 💡 이 오류는 Windows에서 WSL(Windows Subsystem for Linux)
혹은 가상화 기능이 비활성화되어 있을 때 발생합니다.

## 🛠️ 오류 메시지

```
HCS_E_SERVICE_NOT_AVAILABLE
```

## ❓ 원인

이 오류는 다음 사항 중 하나 이상이 활성화되지 않았기 때문에
발생합니다:
  - Windows Subsystem for Linux 기능 비활성화
  - Virtual Maching Platform 기능 비활성화
  - BIOS/UEFI 가상화 기능(Intel VT-x, AMD-V) 비활성화

## ✅ 해결 방법

### 1. 관리자 권한으로 PowerShell 실행

- 시작 메뉴에서 `PowerShell`을 검색
- 마우스 오른쪽 클릭 → **관리자 권한으로 실행**

### 2. 필수 Windows 기능 활성화

PowerShel에 아래 명령어를 하나씩 입력하고 실행하세요.

#### ➤ WSL 기능 활성화

```
dism.exe /online /enable-feature
/featurename:Microsoft-Windows-Subsystem-Linux
/all /norestart
```

#### ➤ 가상 머신 플랫폼 기능 활성화

```
dism.exe /online /enable-feature
/featurename:VirtualMachinePlatform
/all /norestart
```

### 3. 시스템 재부팅

> 위 명령어 실행 후 **PC를 재부팅**하여 변경사항을 적용하세요.

### 4. 가상화 기능 확인(필요할 경우)

PC 재부팅 후에도 동일 오류가 발생한다면 아래 항목을 확인하세요:
- BIOS/UEFI 설정 화면에서 **가상화 기술(Intel VT-x, AMD-V)** 이
**Enabled**로 되어있는지 확인
- 가상화(CPU Virtualization)는 대부분 `Advanced` 또는
`Security` 탭에 있습니다.

✅ **가상화가 꺼져있으면 반드시 `Enabled`로 설정한 후 저장 &
재부팅**하세요.

## 🚀 설치 재시도

위 단계를 완료한 후 콘솔에서 다시 아래 명령어를 실행하세요:

```
wsl --install
```

## 📎 참고

- [Microsoft 공식 WSL 설치 문서](https://learn.microsoft.com/ko-kr/windows/wsl/install)
- PowerShell은 반드시 **관리자 권한으로 실행**해야 합니다.
- Windows 10 2004 이상, Windows 11 권장

## 📌 요약

| 단계 | 설명 |
|------|------|
| 1 | PowerShell을 관리자 모드로 실행 |
| 2 | `dism.exe` 명령어로 필수 기능 활성화 |
| 3 | 시스템 재부팅 |
| 4 | `wsl --install` 재실행 |
| 5 | 필요시 BIOS에서 가상화(VT-x/AMD-V) 설정 확인 |


**작성자**: Limax314

**작성일자**: 2025-07-15

**문서 목적**: WSL 설치 중 발생하는 초기 오류 대응문서 정리