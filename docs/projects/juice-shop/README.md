# OWASP Juice Shop Security Assessment

## 1. Project Overview
 - **Project :** OWASP Juice Shop Security Assessment
 - **Purpose :** 웹 어플리케이션의 보안 구조를 이해하고 취약점 분석 및 검증
 - **Target :** OWASP Juice Shop
 - **Status :** Initial Assessment Completed / BOLA Remediation Pending

 - 본 프로젝트는 OWASP Juice Shop 대상으로 웹 어플리케이션의 보안 구조와 취약점 분석하는 개인 실습 프로젝트
 - 단순한 취약점 탐색에 그치지 않고 HTTP 요청 및 응답 분석, 취약점 재현, 원인 분석, 보안 개선 방안 수립 및 수정 후 검증까지 수행 목표

## 2. Lab Environment
 - **Virtualization :** Oracle VirtualBox
 - **Attacker :** Kali Linux VM
 - **Target :** Ubuntu 24.04 VM
 - **Target Application :** OWASP Juice Shop
 - **Browser :** Firefox
 - **Proxy Tool :** Burp Suite Community Edition
 - **Target IP :** `10.10.10.4`
 - **Attacker IP :** `10.10.10.3`
 - **Target URL :** `http://10.10.10.4:3000/`
 - **Network :** VirtualBox NAT Network
 - 상세 내용: [Environment README](environment/README.md)

## 3. Project Objectives
 - HTTP Request / Response 분석
 - 웹 어플리케이션의 인증 구조 이해
 - 서비스 및 공격 표면 분석
 - 취약점 식별 및 안전한 환경에서 검증
 - 보안 개선 및 재검증

## 4. Assessment Process
 1. Lab Environment Setup
 2. Reconnaissance
 3. HTTP Traffic Analysis
 4. Vulnerability Analysis
 5. Security Verification
 6. Remediation
 7. Final Security Assessment Report
 - 상세 정찰 결과: [Reconnaissance README](reconnaissance/README.md)

## 5. Security Findings

### Finding 01 – Authentication Analysis
 - **Endpoint :** GET /rest/user/whoami?fields=email
 - Cookie token 및 Authorization 헤더에 따른 응답 차이 분석
 - 인증 정보에 따른 사용자 정보 반환 결과 확인
 - **Assessment :** 추가 검증 필요. 인증 우회 취약점으로 확정하지 않음.
 - [Authentication Analysis](vulnerability-analysis/authentication/README.md)

### Finding 02 – Broken Object Level Authorization (BOLA)
 - **Endpoint :** GET /rest/basket/{basketId}
 - 두 개의 테스트 계정을 이용한 사용자 간 접근 권한 검증
 - 다른 사용자 소유의 장바구니 및 상품 정보 반환 확인
 - **Assessment :** 로컬 실습 환경에서 객체 접근 제어 문제 재현
 - **Remediation :** 개선 방안 수립 완료. 실제 코드 수정 및 재검증은 미완료
 - [Access Control Analysis](vulnerability-analysis/access-control/README.md)

### Finding 03 – SQL Injection
 - **Endpoint :** POST /rest/user/login
 - 작은따옴표를 이용한 SQL 구문 변형
 - 로그인 인증 우회 및 토큰 발급 재현
 - SQL 문자열 직접 결합 방식의 문제 확인
 - Sequelize Parameterized Query 적용
 - **Retest :** 정상 로그인 200 OK / 잘못된 비밀번호 401 Unauthorized / 기존 SQL Injection 입력값 401 Unauthorized
 - **Assessment :** 수정 및 해당 테스트 조건의 재검증 완료.
 - [SQL Injection Analysis](vulnerability-analysis/input-validation/sql-injection/README.md)

## 6. Remediation & Verification

### SQL Injection
 - Original Implementation : SQL 문자열에 사용자 입력값 직접 삽입
 - Remediation : Sequelize replacements 적용
 - Normal Login : HTTP 200
 - Invalid Credentials : HTTP 401
 - SQL Injection Payload : 수정 전 HTTP 200 → 수정 후 HTTP 401
 - Result : 기존 테스트 입력값을 통한 인증 우회 차단 확인

### BOLA
 - 사용자 간 장바구니 접근 문제 재현
 - 서버 측 객체 소유권 검증 개선 방안 수립
 - 실제 코드 수정 및 재검증 예정
 - 상세 내용: [Remediation README](remediation/README.md)

## 7. Evidence
 - 각 분석 과정에서 수집한 HTTP 요청 및 응답 화면과 검증 결과를 별도 디렉터리 보관
 - [Authentication Evidence](evidence/authentication/)
 - [Access Control Evidence](evidence/access-control/)
 - [SQL Injection Evidence](evidence/input-validation/sql-injection/)

## 8. Lessons Learned
 - 이번 프로젝트를 통해 하기 내용을 학습 및 검증

 - HTTP 요청과 응답을 기반으로 웹 어플리케이션 동작 분석
 - 사용자 인증과 객체 접근 권한의 차이 이해
 - 객체 ID 변경을 통한 접근 제어 검증
 - SQL Injection의 발생 원인 및 인증 우회 영향 분석
 - 파라미터 바인딩을 통한 SQL Injection 대응
 - 수정 후 동일한 테스트를 통한 보안 개선 효과 검증

## 9. Final Security Assessment Report
 - 프로젝트의 전체 수행 과정, 취약점 분석 결과, 개선 사항 및 평가 한계는 최종 보고서에 정리
 - [Final Security Assessment Report](reports/README.md)

---

 - **Project Scope :** 개인 가상 환경에서 구축한 테스트 시스템  
 - **Project Direction :** Vulnerability Analysis & Penetration Testing  
 - **Portfolio :** BLU2E | RED TEAM LEARNING JOURNEY