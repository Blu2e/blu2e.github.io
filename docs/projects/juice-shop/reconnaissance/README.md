# Reconnaissance

## 1. Overview
 - 본 단계에서는 OWASP Juice Shop을 대상으로 본격적인 취약점 분석에 앞서 대상 시스템의 접근 가능 여부 및 웹 어플리케이션의 기본 구조 파악
 - Kali Linux에서 Ubuntu VM에 구축된 Juice Shop에 접근, Firefox 및 Burp Suite를 활용하여 웹 어플리케이션 요청 및 응답 확인

## 2. Target Identification
 - Target Application: OWASP Juice Shop
 - Target System: Ubuntu VM
 - Target IP: 10.10.10.4
 - Attacker System: Kali Linux
 - Attacker IP: 10.10.10.3
 - Target URL: http://10.10.10.4:3000/

## 3. Port & Service Discovery
 - 대상 시스템의 TCP 3000 포트가 열려 있는 것 확인
 - Kali Linux에서 10.10.10.4:3000으로 접근, 해당 주소에서 웹 어플리케이션에 연결 가능 확인

 - Protocol: TCP
 - Port: 3000
 - Service: OWASP Juice Shop Web Application
 - Accessibility: Accessible

## 4. Web Application Discovery
 - Firefox를 사용하여 대상 웹 어플리케이션 접속
 - 메인 페이지가 정상적으로 표시 확인, 이를 통해 Kali Linux에서 Ubuntu VM에 구축된 Juice Shop 웹 어플리케이션 접근 확인
 - 단, 메인 페이지 접근 성공만으로 모든 API의 접근 가능 여부나 정상 동작 판단은 불가

## 5. HTTP Request & API Discovery
 - Burp Suite Community Edition을 활용하여 웹 어플리케이션의 HTTP 요청 및 응답 확인
 - 분석 과정 API 확인
 - Method: GET
 - Endpoint: /rest/user/whoami?fields=email
 - Function: 사용자 정보 조회
 - Returned Field: email

 - 해당 API는 사용자 이메일 정보 반환 기능과 관련된 것으로 확인
 - API가 존재한다는 사실만으로 취약점이라고 판단하지 않았으며, 인증 정보에 따른 응답 차이와 사용자 정보 접근 조건을 추가로 분석할 필요가 있다고 판단

## 6. Attack Surface Identification
 - 정찰 과정에서 웹 어플리케이션의 주요 기능과 API가 추가 분석 대상이 될 수 있음 확인
 - 주요 분석 관심 영역
 - Authentication: 사용자 인증 및 세션 처리
 - Access Control: 사용자별 데이터 접근 권한
 - Input Validation: 사용자 입력값 처리

 - 각 영역은 이후 취약점 분석 단계에서 별도 검증

## 7. Reconnaissance Findings
 - 이번 정찰 단계에서 확인 내용
 - Kali Linux에서 Ubuntu VM의 TCP 3000 포트에 접근 가능
 - Firefox에서 OWASP Juice Shop 메인 페이지 정상 표시
 - Burp Suite를 통한 HTTP 요청 및 응답 확인
 - 사용자 정보 조회 API 발견
 - 인증, 접근 제어 및 입력값 검증 영역을 후속 분석 대상으로 선정

## 8. Next Analysis
 - 정찰 단계에서 확인한 웹 어플리케이션의 기능과 API를 바탕으로 다음 영역에 대한 취약점 분석 진행
 - Authentication Analysis
 - Access Control Analysis
 - Input Validation Analysis

 - 각 분석 결과와 검증 증거는 vulnerability-analysis/ 및 evidence/ 디렉터리에 별도 기록

## 9. Conclusion
 - 본 단계에서는 대상 시스템의 접근 가능 여부와 웹 어플리케이션의 기본적인 요청 구조 파악
 - 특히 HTTP 요청과 API를 확인함으로써 이후 인증, 접근 제어 및 입력값 검증을 중심으로 취약점 분석을 진행할 수 있는 기초 정보 확보