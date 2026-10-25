# 웹 애플리케이션 정찰 - 진행 기록

## 실습 목적
 - Kali Linux에서 OWASP Juice Shop에 접근해 웹 응답 관찰, 이후 분석할 항목 검색

## 실습 환경
- 공격자: Kali Linux (10.10.10.3)
- 대상 서버: Ubuntu Server (10.10.10.4)
- 대상 애플리케이션: OWASP Juice Shop
- 대상 포트: TCP 3000
- 접속 주소: `http://10.10.10.4:3000`
- 실습 네트워크: RedTeam-Lab (10.10.10.0/24)

## 수행 작업
- Ubuntu Target에 Docker 설치
- OWASP Juice Shop Docker 이미지를 내려받고 컨테이너 실행
- Kali 브라우저에서 Juice Shop 접속
- Nmap으로 대상의 3000번 포트 스캔
- `curl`을 사용해 HTTP 응답 헤더와 HTML 확인
- HTML에서 JavaScript 파일 경로 확인
- `main.js`에서 API로 보이는 경로 추출
- 상품 검색 API에 요청을 보내 JSON 응답 확인

## 확인 결과
- 웹 브라우저에서 Juice Shop 화면 표시 확인
- Nmap에서 `3000/tcp open` 상태 확인
- HTTP 응답 헤더와 HTML을 확인
- 상품 검색 API에서 HTTP 200 응답과 JSON 상품 데이터를 확인

## 결과 해석
- 웹 애플리케이션에 접근할 수 있고 상품 검색 API가 JSON 데이터를 반환하는 것을 확인했습니다.
- 이 결과만으로 취약점이 존재한다고 판단할 수는 없습니다.

## 다음 작업
- API 응답의 상품 가격 필드와 웹 화면에 표시되는 가격 비교
- `main.js`에서 가격 선택 로직이 어떻게 작성되어 있는지 확인
- 클라이언트 코드에서 확인한 동작과 서버 측 처리 여부를 구분해 조사