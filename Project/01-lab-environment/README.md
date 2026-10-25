# Lab Environment

## 실습 목적
 - 가상 환경에서 OWASP Juice Shop을 대상으로 웹 모의해킹 실습을 진행하기 위한 환경 구성

## 실습 환경 개요
- 가상화 프로그램: Oracle VirtualBox
- 공격자: Kali Linux
- 대상 서버: Ubuntu Server
- 웹 실습 대상: OWASP Juice Shop (Docker)
- 실습 네트워크: RedTeam-Lab (NAT Network)
- IPv4 네트워크 대역: 10.10.10.0/24
- Kali Linux IP: 10.10.10.3
- Ubuntu-Target IP: 10.10.10.4
- Juice Shop 접속 주소: http://10.10.10.4:3000

## Ubuntu-Target 구축 기록

### 1. 구축 목적
- Juice Shop을 실행하고 Kali Linux에서 접근할 대상 서버 구성

### 2. 사용 환경
- 가상 머신 이름: Ubuntu-Target
- 운영체제: Ubuntu Server 24.04.5 LTS
- 가상 디스크: VDI, 최대 30GB (동적 할당)
- 메모리: 4GB
- CPU: 2개
- 네트워크 인터페이스: enp0s3
- 실습 네트워크 IPv4: 10.10.10.4/24

### 3. 설치 과정
- VirtualBox에 Ubuntu-Target 가상 머신 생성
- Ubuntu Server 설치 및 사용자 계정 생성
- OpenSSH Server 설치
- 설치 완료 후 재부팅 및 로그인 확인
- 초기 설치 완료 후 복구용 스냅샷 생성

### 4. 문제 발생 및 해결
- 발생한 문제: Ubuntu 설치 후 로그인 실패
- 원인: 설치 과정에서 사용자 계정명이 `bule`로 생성되었으나 로그인할 때 `blue`를 입력해 계정명이 일치하지 않음
- 확인 과정: 복구 모드에서 실제 계정명을 확인하고 계정명을 수정한 뒤 다시 로그인
- 결과: Ubuntu Server 로그인 성공

## Kali Linux 구축 기록

### 1. 구축 목적
- 웹 애플리케이션을 조사하고 테스트할 공격자 측 가상 머신 구성

### 2. 사용 환경
- 가상 머신 이름: kali-linux-2026.2-virtualbox-amd64
- 운영체제: Kali Linux 2026.2
- 가상화 프로그램: Oracle VirtualBox
- 네트워크 인터페이스: eth0
- 실습 네트워크 IPv4: 10.10.10.3/24

### 3. 설치 과정
- 공식 VirtualBox 이미지 다운로드 및 등록
- Kali Linux 최초 부팅 및 로그인
- 기본 비밀번호 변경
- 네트워크 설정 및 IP 주소 확인
- 초기 설정 완료 후 복구용 스냅샷 생성

## 가상 네트워크 및 연결 확인
- 네트워크 이름: RedTeam-Lab
- 네트워크 방식: NAT Network
- IPv4 네트워크 대역: 10.10.10.0/24
- Kali Linux: 10.10.10.3/24
- Ubuntu-Target: 10.10.10.4/24

## OWASP Juice Shop 실행 환경
- Ubuntu Target에 Docker 설치
- OWASP Juice Shop Docker 이미지를 내려받고 컨테이너 실행
- Kali Linux 브라우저에서 `http://10.10.10.4:3000` 접속 성공
- Nmap에서 3000/tcp가 open 상태인 것을 확인

## 다음 단계
- 웹 애플리케이션의 HTTP 응답과 API를 조사
- 조사한 결과 중 추가 분석할 항목 선정
- 확인한 사실과 아직 검증하지 않은 사항을 구분하여 기록