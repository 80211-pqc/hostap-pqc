# hostap-pqc

무선 네트워크(IEEE 802.11) 환경을 위한 **양자내성암호(PQC) 프로토콜 구현체**입니다.
[w1.fi hostap](https://w1.fi/hostap.git) 오픈소스 프로젝트를 기반으로, 새로 제안·명세화한
PQC 키 교환 및 암호화 과정을 도입하여 **AP와 STA 양측**에서 프로토콜을 지원합니다.


## 목차

- [프로젝트 소개](#프로젝트-소개)
- [프로젝트 구성](#프로젝트-구성)
- [상태](#상태)
- [기반 (Upstream)](#기반-upstream)
- [의존성](#의존성)
- [빌드 & 설치](#빌드--설치)
- [라이선스](#라이선스)
- [보안](#보안)
- [기여](#기여)

## 프로젝트 소개

hostap-pqc는 기존 hostapd/wpa_supplicant의 인증·키 교환 흐름에 양자내성암호를 결합하여,
양자 컴퓨팅 환경에서도 안전한 무선 네트워크 연결을 목표로 합니다.

- AP(Access Point)와 STA(Station) 양측에서 PQC 프로토콜을 지원합니다.
- 키 교환 로직 및 암호화 과정에 PQC 알고리즘을 새로 도입합니다.
- PQC 연산은 직접 구현하지 않고 **liboqs-80211**의 코드를 호출하여 사용합니다.

## 프로젝트 구성

80211-PQC 프로젝트는 다음 4개 레포로 구성됩니다.

| 레포 | 역할 |
|------|------|
| 프로토콜 명세서 | 제안 및 표준 기반 상세 구현 명세 <!-- TODO: 링크 --> |
| **hostap-pqc** ✅ | 프로토콜 실 구현체 |
| [openwrt-pqc](https://github.com/80211-PQC/openwrt-pqc) | 구현된 프로토콜을 적용한 펌웨어 |
| [liboqs-80211](https://github.com/80211-PQC/liboqs-80211) | 802.11 환경에 맞게 최적화된 liboqs 연산 구현체 |

## 상태

> ⚠️ **실험적 / 연구용 (Experimental)**
> 본 프로젝트는 연구 및 표준 제안을 목적으로 하는 실험적 구현입니다.
> 프로덕션 환경에서 사용하지 마십시오. 인터페이스와 프로토콜은 예고 없이 변경될 수 있습니다.

## 기반 (Upstream)

- **원본 프로젝트:** hostap (hostapd / wpa_supplicant) — <https://w1.fi/hostap.git>
- **Base 스냅샷:** `ca266cc24d8705eb1a2a0857ad326e48b1408b20`
  - 이 스냅샷은 OpenWrt 25.12 펌웨어가 사용한 hostapd 버전과 동일합니다.
- **원본 라이선스:** BSD-3-Clause (레포 내 `COPYING` 및 `README.hostap` 참조)
- 80211-PQC의 수정분은 위 Base 스냅샷을 기준으로 합니다.


## 의존성

- **[liboqs-80211](https://github.com/80211-PQC/liboqs-80211)** — PQC 연산 라이브러리
  - OpenWrt 빌드 시 `openwrt-pqc/package/custom/liboqs-opt/Makefile`이 실행되어
    liboqs-80211을 지정된 버전으로 받아 빌드하고, hostapd/wpa_supplicant가
    링크할 라이브러리로 설치합니다.
  - 따라서 이 의존성은 openwrt-pqc 빌드 과정에서 자동으로 처리됩니다.

## 빌드 & 설치

openwrt-pqc에서 hostap-pqc 패키지를 clone 하여 사용하므로 해당 패키지를 따로 빌드, 설치할 필요가 없습니다.


## 라이선스

본 프로젝트는 원본 hostap과 동일하게 **BSD-3-Clause**로 배포됩니다.
원본 저작권 표시를 유지하며, 수정분에 대한 저작권을 추가합니다.

```
Copyright (c) 2002-2022, Jouni Malinen <j@w1.fi> and contributors
Copyright (c) 2026, 80211-PQC
```

- 원본 라이선스 전문은 레포 내 `COPYING` 및 `README.hostap`의 License 섹션을 참조하십시오.
- 새로 작성한 소스 파일에는 `// SPDX-License-Identifier: BSD-3-Clause` 헤더를 포함합니다.

## 보안

보안 취약점은 **공개 이슈로 등록하지 마십시오.**
취약점 제보 절차와 정책은 [`SECURITY.md`](./SECURITY.md)를 참조하십시오.

- 제보 채널: `rlagh1073@naver.com`

## 기여

기여를 환영합니다. 기여 범위 구분, 워크플로우, 커밋 규칙은
[`CONTRIBUTING.md`](./CONTRIBUTING.md)를 참조하십시오.

- 방식: **Fork & Pull Request**
- 모든 커밋에 **`Signed-off-by`** 서명 필요 (DCO)
- hostap 자체에 대한 개선은 원본 프로젝트([w1.fi](https://w1.fi/hostap.git))에 제보해 주십시오.
