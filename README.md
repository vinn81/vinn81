<div align="center">

# 김가빈 · Kim Gabin

**네트워크 · 시스템 보안 엔지니어 지망생**

망을 나누고 경계를 통제하며, 탐지·관제를 직접 구축해 결과 화면으로 검증합니다.<br/>
장애가 생기면 재설치보다 원인 범위를 좁혀 해결합니다.

[![Portfolio](badges/portfolio.svg)](https://intro-my-nine.vercel.app/)

</div>

---

### 🧭 지원 분야

`네트워크 보안 엔지니어` · `보안관제(SOC)` · `시스템·인프라 엔지니어`

- 🎓 영남대학교 컴퓨터학부 재학 중
- 🛡️ SKT ALEPH AI 보안·인프라 과정 (2026.08 ~ 2026.11)
- 📜 정보처리기사 취득 · 리눅스마스터 2급 준비 중

---

### 📊 결과로 확인한 것

| 결과 | 무엇을 했나 | 기술 | 근거 |
| :---: | --- | --- | --- |
| **4 / 4**<br/>**3 / 3** | **Half-Security Labs (혼자 구축)** · VLAN 3개와 Cisco ASAv로 Inside·DMZ·Outside 분리. Wazuh Agent 4대 Active·24시간 이벤트 384건 조회, 직접 쓴 Snort 3 룰로 Kali의 ping·nmap·웹 접속 모두 경보 | GNS3 · ASAv · Snort 3 · Wazuh · OSSEC · OpenVPN | [결과보고서 PDF](https://intro-my-nine.vercel.app/works/half-security-labs.pdf) · [포트폴리오](https://intro-my-nine.vercel.app/#works) |
| **10 / 10** | **제로 트러스트 보안 Agent 「헤임달」** · 사용자·기기·세션 확인, 업무 앱 접근 판정, XDR 경보, 내 PC 보안 상태 실측(Windows 기본 도구와 대조). 단계별 코드 검토 전부 통과 | JavaScript · PowerShell | [저장소](https://github.com/vinn81/zero-trust-security-agent) · [상세 페이지](https://intro-my-nine.vercel.app/defense/) |
| **401** | **포트폴리오 사이트 · 패스키 로그인** · 서버에서 challenge·origin·서명 검증, HMAC 서명 HttpOnly 세션. 로그인 없는 비공개 API 요청이 401로 거절되는 것 확인 | JavaScript · Vercel Functions · Neon PostgreSQL | [저장소](https://github.com/vinn81/introMy) · [구현 기록](https://github.com/vinn81/introMy/blob/main/AUTH_IMPLEMENTATION.md) |
| **403** | **WAF 실습** · Apache + ModSecurity 룰로 `/admin` 접근과 XSS 요청 차단, `modsec_audit.log`로 확인 | Apache · ModSecurity | SKT ALEPH 실습 노트 (비공개) |
| **완주** | **CTF 머신 「OISecure 업무 포털」 제작 (팀)** · 디렉터리 열거 → UNION SQLi로 SSH 자격 증명 탈취 → sudo 파이썬 모듈 하이재킹 권한 상승. 공격 경로 외에는 Prepared Statement·CSRF·bcrypt·최소 권한으로 막음 | Ubuntu · PHP · MariaDB | SKT ALEPH 과제 (비공개, 풀이 유출 방지) |
| **8개 도메인** | **진로온(JOBE) 종합설계 백엔드** · 전공 진단 서비스 REST API, 회원가입·refresh token·이메일 인증 | Java · Spring Boot · JPA | [저장소](https://github.com/vinn81/JOBE) |
| **매일 기록** | **달구벌 NOW** · 달구벌대로 일별 통행속도를 수집해 전날과 비교 | Python · PostgreSQL | [저장소](https://github.com/vinn81/aleph-traffic) · [사이트](https://aleph-traffic.vercel.app/) |
| **팀 배정** | **Automatch** · 풋살 매치 예약과 자동 팀 배정 | Java · Spring Boot · JPA · MySQL | [저장소](https://github.com/vinn81/OS-Project) |

---

### 🌐 Half-Security Labs — 혼자 구축한 망

```mermaid
flowchart LR
    subgraph OUT[Outside · /26 VLAN 10·20·30]
        K[Kali 공격자]
        IDS[Snort 3 NIDS]
    end
    K -->|ping · nmap · HTTP| FW{{Cisco ASAv}}
    K -. 경보 .-> IDS
    FW --> DMZ
    FW --> IN
    subgraph DMZ[DMZ]
        DNS[DNS]
        WEB[Web · DB]
        OS[OSSEC Server]
        ZB[Zabbix]
        WZ[Wazuh]
        PMM[PMM]
    end
    subgraph IN[Inside]
        PF[pfSense · OpenVPN]
        EA[Earth]
    end
```

<details>
<summary><b>재설치 없이 해결한 트러블슈팅 3건</b></summary>

| 증상 | 원인 | 해결 |
| --- | --- | --- |
| rsyslog가 기동되지 않아 로그를 DB에 모을 수 없음 | 수업 자료의 구형 지시자 설정이 설치 버전의 모듈 선언 방식과 맞지 않음 | 설정 → 서비스 → DB 순서로 점검, 모듈 선언 방식으로 다시 써서 기동. LogAnalyzer에서 DNS 로그 확인 |
| pfSense OpenVPN 클라이언트 내보내기 인증서 오류 | OpenVPN 서버가 웹 관리용 자체 서명 인증서를 사용 | OpenVPN 전용 CA·인증서로 교체, VPN 주소 할당·터널 연결 확인 |
| Wazuh와 OSSEC 설치 충돌 | 둘 다 `/var/ossec` 경로 사용 | 덮어쓰지 않고 OSSEC Server는 별도 VM, Wazuh Agent는 서버 4대로 역할 분리 |

</details>

---


### 🔐 Security Practice

<details>
<summary><b>탐지 · 관제 · 방어</b></summary>

- **IDS/IPS**: Snort 3 · Suricata 사용자 정의 룰 작성
- **HIDS / SIEM**: Wazuh Agent 연동, OSSEC 변조 탐지, Splunk 설치
- **모니터링**: Zabbix(에이전트 관제·팀 Wazuh 서버 연동), Nagios Core 소스 빌드, PMM
- **WAF**: Apache + ModSecurity `SecRule` 작성(`REQUEST_URI`, `ARGS` 검사 → 403), `modsec_audit.log` 확인
- **로그 분석**: rsyslog → LogAnalyzer, GoAccess 실시간 HTML 리포트
- **방화벽·VPN**: Cisco ASAv Inside·DMZ·Outside 분리, ACL·DACL, pfSense OpenVPN

</details>

<details>
<summary><b>모의해킹 · CTF</b></summary>

- **Metasploitable2**: nmap 서비스 식별 → vsftpd 2.3.4 · UnrealIRCd · Samba usermap · VNC · PostgreSQL 취약점 검증, WebDAV 업로드, SMTP VRFY 사용자 열거
- **CTF 머신 풀이**: Momentum2(쿠키 브루트포스 · Burp 업로드 우회 · 권한 상승), DoubleTrouble2, qdPM 등 9회 실습 기록
- **웹 취약점**: OWASP Juice Shop 단계별 공략, bWAPP A1~A10 정리 (SQLi · XSS · LFI/RFI · 커맨드 인젝션 · 파일 업로드)
- **위협 분석 프레임워크**: Cyber Kill Chain, MITRE ATT&CK (Caldera · Atomic Red Team 에뮬레이션 실습)

</details>

<details>
<summary><b>네트워크 공격 원리 (학부 네트워크 보안·블록체인)</b></summary>

SYN 플러딩 · TCP Reset · 세션 하이재킹 · Mitnick 공격 · DNS 캐시 감염 · Kaminsky 공격 · DNS 리바인딩 · 머클 트리 · 합의 알고리즘

</details>

---

### 🧰 Skills

**Network & Security**

![Cisco](badges/cisco.svg)
![GNS3](badges/gns3.svg)
![pfSense](badges/pfsense.svg)
![Snort](badges/snort.svg)
![Suricata](badges/suricata.svg)
![Wazuh](badges/wazuh.svg)
![OSSEC](badges/ossec.svg)
![Zabbix](badges/zabbix.svg)
![Splunk](badges/splunk.svg)
![ModSecurity](badges/modsecurity.svg)
![Wireshark](badges/wireshark.svg)

VLAN · EIGRP/OSPF · HSRP · ACL · NAT · Site-to-Site VPN · OpenVPN · DMZ 설계

**Offensive (탐지 룰 검증용)**

![Kali](badges/kali.svg)
![Nmap](badges/nmap.svg)
![Metasploit](badges/metasploit.svg)
![Burp Suite](badges/burp-suite.svg)

**Server & OS**

![Linux](badges/linux.svg)
![Apache](badges/apache.svg)
![MariaDB](badges/mariadb.svg)
![Docker](badges/docker.svg)
![PowerShell](badges/powershell.svg)
![Raspberry Pi](badges/raspberry-pi.svg)

DNS · HTTPS · SFTP/FTP · NFS · Samba · rsyslog · cron · 권한·umask · LVM/RAID

**Programming**

![Python](badges/python.svg)
![Java](badges/java.svg)
![Spring Boot](badges/spring-boot.svg)
![JavaScript](badges/javascript.svg)
![PHP](badges/php.svg)
![C](badges/c.svg)
![Shell](badges/shell.svg)
![SQL](badges/sql.svg)
![Flask](badges/flask.svg)

---

### 📚 학습 기록

| 분야 | 내용 |
| --- | --- |
| 전공 | 컴퓨터네트워크 · 운영체제 · 데이터베이스 · 컴퓨터 구조 · 알고리즘 · 소프트웨어 공학 · 웹프로그래밍 · 인공지능 · 컴퓨터 비전 · 데이터 분석 · IoT(라즈베리파이 GPIO · I2C · SPI · UART · MQTT) |
| 자격 | 정보처리기사 필기 노트(보안 심화 · SW 설계와 보안 · DB · 네트워크/OS), 리눅스마스터 2급 정리노트(23장) |
| 개인 공부 | Spring MVC · HTTP |

<div align="center">

<sub>모든 수치는 결과보고서와 실측 화면에 근거합니다 · 자세한 내용은 <a href="https://intro-my-nine.vercel.app/">포트폴리오</a></sub>

</div>
