# OWASP Juice Shop – Red Team Security Assessment

## 1. Project Overview
 - 본 프로젝트는 OWASP Juice Shop을 대상으로 웹 어플리케이션의 보안 취약점을 분석, 실제 공격 시나리오를 통해 보안 문제 검증 목적
 - 단순한 취약점 탐색에 그치지 않고 HTTP 요청 및 응답 분석, 취약점 재현, 원인 분석, 보안 개선 방향 수립 및 수정 후 검증 수행

 - Project / OWASP Juice Shop Security Assessment
 - Assessment Type / Web Application Security Testing
 - Target / OWASP Juice Shop
 - Environment / Local Virtualized Lab
 - Main Focus / Authentication, Access Control, Input Validation

## 2. Testing Environment
 - Target Application / OWASP Juice Shop
 - Target URL / http://10.10.10.4:3000
 - Operating System / Ubuntu 24.04
 - Container / Docker
 - Testing Platform / Kali Linux
 - Testing Tool / Burp Suite Community Edition
 - Network Environment / VirtualBox Local Lab

 - 모든 테스트는 직접 구성한 로컬 실습 환경에서 수행

## 3. Assessment Scope

### 3.1 Reconnaissance
 - 대상 어플리케이션 및 실행 환경 확인
 - 서비스 포트 및 접근 가능한 엔드포인트 분석
 - HTTP 응답 헤더 및 서버 응답 확인
 - 어플리케이션 구조와 테스트 범위 파악

 - Detailed Report : projects/juice-shop/reconnaissance/

### 3.2 Authentication Analysis
 - 로그인 API 요청 및 응답 분석
 - Cookie token과 Authorization 헤더 동작 비교
 - 인증 정보 변경에 따른 사용자 정보 조회 결과 확인
 - HTTP 상태 코드와 실제 응답 데이터 비교

 - Detailed Report : projects/juice-shop/vulnerability-analysis/authentication/README.md

### 3.3 Access Control – BOLA / IDOR
 - 두 개의 테스트 계정 생성
 - 사용자별 User ID 및 Basket ID 확인
 - 다른 사용자 장바구니 ID를 이용한 교차 접근 테스트
 - 다른 사용자 소유의 장바구니 및 상품 정보 반환 확인

 - Detailed Report : projects/juice-shop/vulnerability-analysis/access-control/README.md

### 3.4 SQL Injection
 - 로그인 API의 SQL Injection 취약점 분석
 - SQL 구문 오류 및 인증 우회 재현
 - SQL 쿼리 문자열 직접 삽입 방식의 문제점 확인
 - Sequelize Parameterized Query 적용
 - 수정 전후 동일한 공격 입력값으로 검증

 - Detailed Report : projects/juice-shop/vulnerability-analysis/input-validation/sql-injection/README.md

## 4. Remediation & Verification
 - SQL Injection 분석 과정에서 확인한 취약한 로그인 쿼리를 파라미터 바인딩 방식으로 수정

 - Original Implementation / SQL 문자열에 사용자 입력값 직접 삽입
 - Remediation / Sequelize replacements 적용
 - Normal Login / 200 OK
 - Invalid Credentials / 401 Unauthorized
 - SQL Injection Payload / 수정 전 200 OK → 수정 후 401 Unauthorized

 - 동일한 공격 입력값으로 재검증하여 인증 우회 차단 확인

## 5. Evidence
 - 각 분석 과정에서 수집한 HTTP 요청 및 응답 화면과 검증 결과를 별도 디렉터리 보관

 - Reconnaissance / projects/juice-shop/evidence/
 - Authentication / projects/juice-shop/evidence/authentication/
 - Access Control / projects/juice-shop/evidence/access-control/
 - SQL Injection / projects/juice-shop/evidence/input-validation/sql-injection/

## 6. Security Findings Summary

### Finding 01 – Authentication Analysis
 - Analysis / Cookie token 및 Authorization 헤더 동작 비교
 - Result / 인증 정보 변경에 따른 사용자 정보 조회 결과 차이 확인
 - Assessment / 추가 검증 필요, 취약점 확정 판정 보류

### Finding 02 – Broken Object Level Authorization
 - Analysis / 다른 사용자의 장바구니 접근 시도
 - Result / 교차 사용자 장바구니 및 상품 정보 반환 확인
 - Assessment / 로컬 실습 환경에서 접근 제어 문제 재현

### Finding 03 – SQL Injection
 - Analysis / 로그인 API의 SQL Injection
 - Result / 인증 우회 재현 성공
 - Remediation / Parameterized Query 적용
 - Verification / 동일 페이로드 요청에서 401 Unauthorized 확인

## 7. Skills & Technologies
 - Web Application Security Testing
 - HTTP Request / Response Analysis
 - Authentication Analysis
 - Broken Object Level Authorization (BOLA)
 - Insecure Direct Object Reference (IDOR)
 - SQL Injection
 - Parameterized Query
 - Burp Suite Repeater
 - Docker
 - Linux
 - Git / GitHub

## 8. Lessons Learned
 - 이번 프로젝트를 통해 보안 분석 역량 실습

 - HTTP 요청과 응답을 기반으로 어플리케이션 동작 분석
 - 사용자 인증과 객체 접근 권한의 차이 이해
 - 객체 ID 변경을 통한 접근 제어 검증
 - SQL Injection의 발생 원인 및 인증 우회 영향 분석
 - 파라미터 바인딩을 통한 SQL Injection 대응
 - 수정 후 동일한 테스트를 통한 보안 개선 효과 검증

## 9. Project Conclusion
 - 본 프로젝트에서는 OWASP Juice Shop을 대상으로 웹 어플리케이션의 인증, 접근 제어 및 입력값 처리 영역 분석
 - 취약점 발견과 재현, 코드 수준의 개선 및 수정 후 검증까지 수행, 웹 어플리케이션 보안 테스트의 전체 흐름 경험
 - 본 프로젝트는 로컬 실습 환경에서 수행한 보안 분석 결과, 실제 운영 서비스에 대한 침투 테스트 결과 의미 아님