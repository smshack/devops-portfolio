
```
[인터넷 유저] 
     │ (https://kibana.smartseoapp.com)
     ▼
┌──────────────┐
│  Nginx Web   │
└──────┬───────┘
       │ (Docker 내부 네트워크: 초고속/안전)
       ▼
┌──────────────┐      ┌──────────────┐
│    Kibana    ├──────▶Elasticsearch │ (외부 노출 X)
└──────────────┘      └──────▲───────┘
                             │
                      ┌──────┴───────┐
                      │   Logstash   │ (외부 노출 X)
                      └──────────────┘
```

### Elasticsearch를 Nginx로 열면 안 되는 이유
- Elasticsearch는 모든 로그 데이터가 저장되는 DB(데이터베이스)
- 만약 이를 elasticsearch.smartseoapp.com 같은 도메인으로 외부에 노출하면, 해커들이 자동화 봇을 돌려 온갖 공격을 감행
- 실제로 보안 설정을 대충 하고 외부로 열어둔 Elasticsearch DB가 통째로 랜섬웨어에 걸려 인덱스가 모두 삭제되는 사고가 현업에서 굉장히 자주 일어남 
- 외부 접속은 Kibana 화면으로만 하고, Elasticsearch는 오직 Docker 내부 네트워크(services) 안에서만 소통하게 두어야 안전

### Logstash는 웹서버가 아님
- Logstash는 웹 브라우저로 접속하는 대시보드가 아니라, 백엔드 서버나 Filebeat 같은 수집기가 던져주는 로그 데이터를 받아먹는 파이프라인 엔진
- 따라서 Nginx 같은 HTTP 웹 서버 프록시 뒤에 둘 필요가 전혀 없음

### 요약
Nginx 역방향 프록시 주소 부여: Kibana 하나만 처리 (사람이 접속해야 하므로)

내부 네트워크 격리: Elasticsearch (DB이므로 절대 도메인 주소 주지 말 것)

포트 다이렉트 오픈 + 방화벽 차단: Logstash (다른 서버에서 로그 수집 장치들이 접근해야 할 때만 IP 방화벽 오픈)