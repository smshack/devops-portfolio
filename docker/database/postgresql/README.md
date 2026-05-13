# PostgreSQL Docker Compose 구성

Docker Compose를 사용하여 PostgreSQL 컨테이너를 실행하는 예제입니다.
데이터는 호스트 서버의 `/storage/docker-storage/postgres/data` 경로에 저장되며, Docker Volume 이름(`postgres_data`)으로 관리됩니다.

---

# 구성 파일

```yaml
services:
  postgres:
    image: postgres:latest
    container_name: postgres
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
      POSTGRES_DB: testdb
    ports:
      - "5432:5432"
    volumes:
      - postgres_data:/var/lib/postgresql/data
    networks:
      - services

volumes:
  postgres_data:
    name: postgres_data
    driver: local
    driver_opts:
      type: none
      o: bind
      device: /storage/docker-storage/postgres/data

networks:
  services:
    external: true
```

---

# 디렉토리 생성

PostgreSQL 데이터 저장 경로를 미리 생성합니다.

```bash
mkdir -p /storage/docker-storage/postgres/data
```

---

# Docker Network 생성

해당 compose는 외부 네트워크(`services`)를 사용합니다.

최초 1회 생성:

```bash
docker network create services
```

확인:

```bash
docker network ls
```

---

# 컨테이너 실행

```bash
docker compose up -d
```

실행 상태 확인:

```bash
docker ps
```

---

# PostgreSQL 접속 확인

컨테이너 내부 접속:

```bash
docker exec -it postgres bash
```

PostgreSQL 접속:

```bash
psql -U user -d testdb
```

테이블 목록 확인:

```sql
\dt
```

종료:

```sql
\q
```

---

# 볼륨 확인

Docker Volume 목록 확인:

```bash
docker volume ls
```

상세 정보 확인:

```bash
docker volume inspect postgres_data
```

---

# 데이터 저장 위치

실제 데이터는 아래 경로에 저장됩니다.

```bash
/storage/docker-storage/postgres/data
```

컨테이너 삭제 후에도 데이터는 유지됩니다.

---

# 컨테이너 중지 및 삭제

중지:

```bash
docker compose stop
```

삭제:

```bash
docker compose down
```

주의:

* `down`을 수행해도 호스트 데이터(`/storage/docker-storage/postgres/data`)는 삭제되지 않습니다.
* `docker volume rm postgres_data` 실행 시 볼륨 연결만 제거됩니다.

---

# 권한 문제 발생 시

PostgreSQL 권한 오류가 발생하면 아래 명령 실행:

```bash
chown -R 999:999 /storage/docker-storage/postgres/data
```

---

# 추천 디렉토리 구조

```text
/storage/docker-storage/
└── postgres/
    └── data/
```

---

# 운영 시 권장 사항

운영 환경에서는 아래 항목 추가를 권장합니다.

* PostgreSQL 버전 고정

  ```yaml
  image: postgres:16
  ```

* `.env` 파일로 계정 정보 분리

* 백업 스크립트 구성

* Healthcheck 추가

* 모니터링(exporter + Prometheus + Grafana) 구성

* WAL 아카이브 및 Replication 구성 가능
