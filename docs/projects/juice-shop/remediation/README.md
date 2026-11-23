# Remediation

## 1. Overview
 - 본 문서는 OWASP Juice Shop 취약점 분석 과정에서 확인한 보안 문제 개선 방안 및 검증 결과 기록
 - 취약점의 근본 원인 분석, 적절한 보안 대응 방안 제시, 실제 수정이 가능한 항목에 대해서는 수정 후 재검증 수행
 - 본 프로젝트에서는 SQL Injection과 Broken Object Level Authorization(BOLA)을 중심으로 개선 사항 정리

## 2. SQL Injection

### 2.1 Vulnerability Overview
 - 로그인 API의 SQL 쿼리에서 사용자 입력값이 문자열 결합 방식으로 사용되는 것 확인
 - 이로 인해 입력값이 SQL 구문의 일부로 해석될 가능성 존재, 인증 과정에서 SQL Injection 발생 검증

### 2.2 Root Cause
 - 기존 코드는 사용자 입력값을 SQL 쿼리에 직접 삽입하는 구조

**Before**
models.sequelize.query(`SELECT * FROM Users WHERE email = '${req.body.email || ''}' AND password = '${security.hash(req.body.password || '')}' AND deletedAt IS NULL`, { model: UserModel, plain: true })

 - 사용자 입력값과 SQL 구문 미분리로 SQL Injection에 취약한 구조

### 2.3 Remediation
 - Sequelize의 replacements 기능을 사용하여 SQL 쿼리와 사용자 입력값 분리

**After**
models.sequelize.query("SELECT * FROM Users WHERE email = :email AND password = :password AND deletedAt IS NULL", { replacements: { email: req.body.email || "", password: security.hash(req.body.password || "") }, model: UserModel, plain: true })

 - 개선 사항
 - SQL 쿼리와 사용자 입력값 분리
 - Parameterized Query 적용
 - 사용자 입력값이 SQL 구문으로 해석되는 문제 방지

### 2.4 Verification
 - 수정 이후 동일한 테스트 조건에서 인증 동작 재검증
 - 정상 계정 로그인: HTTP 200
 - 잘못된 비밀번호: HTTP 401
 - SQL Injection 테스트 입력값: HTTP 401

 - 기존에는 SQL Injection 입력값으로 인증 토큰이 발급, 수정 이후에는 인증 거부 확인

### 2.5 Result
 - Parameterized Query 적용 이후 기존에 재현되었던 SQL Injection 공격 차단 확인

## 3. Broken Object Level Authorization (BOLA)

### 3.1 Vulnerability Overview
 - 사용자 B가 사용자 A의 장바구니 데이터에 접근할 수 있는 문제 확인
 - 사용자별 장바구니 접근 권한 검증이 충분하지 않은 것으로 판단

### 3.2 Root Cause
 - 장바구니 조회 요청에서 객체 ID를 변경 시 다른 사용자의 데이터가 반환되는 현상 확인
 - 서버 측 객체 소유권 검증이 적절하게 적용되지 않았을 가능성 확인

### 3.3 Recommended Remediation
 - 개선 방안 권고
 - 서버 측에서 요청 사용자와 객체 소유자 일치 여부 검증
 - 객체 조회 시 사용자 식별 정보를 함께 적용
 - 권한이 없는 객체에 대한 접근 차단
 - 권한 검증 실패 시 적절한 HTTP 응답 반환

 - 예를 들어 장바구니 조회 시 요청한 사용자의 ID와 장바구니 소유자의 ID를 비교하는 서버 측 권한 검증 필요

### 3.4 Verification Status
 - 현재 BOLA 취약점은 재현 및 영향 확인 완료
 - 다만 실제 소스 코드 수정과 수정 후 재검증은 아직 수행하지 않았으므로 개선 완료로 판단하지 않음

## 4. Security Improvement Summary

### SQL Injection
 - 기존 취약한 SQL 쿼리 구조 확인
 - Sequelize replacements 적용
 - 수정 후 정상 및 비정상 로그인 검증
 - SQL Injection 재현 차단 확인

### BOLA
 - 사용자 간 장바구니 접근 문제 확인
 - 서버 측 객체 소유권 검증 필요성 확인
 - 개선 방안 정리
 - 실제 수정 및 재검증은 향후 작업으로 분류

## 5. Lessons Learned
 - 이번 개선 과정을 통해 취약점 발견뿐만 아니라 근본 원인을 파악, 적절한 대응 방안을 적용하는 과정의 중요성 확인
 - 특히 SQL Injection 분석에서는 소스 코드 수정만으로 개선이 완료되는 것이 아니라, 실제 어플리케이션이 실행하는 코드에 수정 사항 반영 확인하고 동일한 조건으로 재검증해야 한다는 점을 경험
 - 또한 BOLA 분석을 통해 사용자 인증과 객체 접근 권한 검증은 별개 보안 요소, 인증된 사용자라도 다른 사용자의 데이터에 접근할 수 없도록 서버 측 권한 검증 필요하다는 점을 확인

## 6. Conclusion
 - 본 프로젝트에서는 SQL Injection 취약점에 대해 실제 코드 개선과 재검증 수행, BOLA 취약점에 대해서는 원인 분석과 개선 방안 정리
 - 향후에는 객체 접근 제어 개선 및 재검증을 추가 진행, 취약점 발견부터 개선까지의 전체 과정 확장 예정