# Lab Environment

## 1. Environment Overview
 - 본 프로젝트는 Oracle VirtualBox 기반 가상 환경에서 OWASP Juice Shop을 대상으로 웹 어플리케이션의 보안 구조와 취약점 분석 진행
 - Ubuntu VM을 테스트 대상으로 구성, Kali Linux VM에서 Firefox와 Burp Suite를 활용하여 웹 어플리케이션 요청 및 응답 분석

## 2. Lab Components
 - Virtualization: Oracle VirtualBox
 - Attacker System: Kali Linux VM
 - Target System: Ubuntu VM
 - Target Application: OWASP Juice Shop
 - Proxy Tool: Burp Suite Community Edition
 - Web Browser: Firefox

## 3. Target Information
 - Application: OWASP Juice Shop
 - Target URL: http://10.10.10.4:3000/
 - Target IP: 10.10.10.4
 - Container Name: juice-shop
 - Docker Image: bkimminich/juice-shop
 - Application Version: 20.3.0

## 4. Network Information
 - Kali Linux IP: 10.10.10.3
 - Ubuntu IP: 10.10.10.4
 - Virtualization Platform: Oracle VirtualBox

## 5. Testing Tools
 - Firefox: 웹 어플리케이션 접속 및 기능 테스트
 - Burp Suite Community Edition : HTTP Request / Response 가로채기 및 분석

## 6. Security Testing Scope
 - Target: 개인 가상 환경에 구축한 OWASP Juice Shop
 - Objective: 웹 어플리케이션 취약점 분석 및 검증
 - Scope: Authentication, Access Control, Input Validation
 - Methodology: 수동 HTTP 트래픽 분석 및 허가된 환경에서의 취약점 검증

## 7. Environment Notes
 - 본 프로젝트는 개인 가상 환경에서 구축한 테스트 시스템 대상으로 수행, 외부 운영 시스템이나 제3자의 서비스는 테스트 대상으로 사용하지 않음