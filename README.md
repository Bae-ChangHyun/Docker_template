# docker_env

## Dockerfile Setting

- Install necessary packages
- Install postgresql client(VER=13)
- Install redis client
- Install miniconda and create "dev" env
- Install Python(VER=3.10), jupyter-notebook, pytorch(VER=1.13/CUDA=11.7)

## Docker-compost Setting

- Redhat
    - Build image based on Dockerfile on same directory
    - Set port(default:22)
    - Mount directory- project dir, postgresql data dir
        : 여기서 로컬 혹은 서버의 디렉토리를 연동해놔야 컨테이너를 삭제해도 데이터가 사라지지 않음, 폴더 확인가능
    - Set environment variable
    - Connect with postgres container
    - Connect network
- Postgres
    - Pull image 
    - Set port
    - Set environment variable
        : db명, db 유저, db 비밀번호
    - Mount directory - postgresql data dir
        :반드시 마운트해야함
    - Connect network
- redis
    - Pull image 
    - Set port(default:6379)
    - Connect network
- pgadmin4
    - Pull image 
    - Set port(default:8080)
    - Set environment variable
        : 계정명, 비밀번호
    - Connect network

## How to

1. dockerfile에서 redhat 서버에 기본 패키지들을 설치하고, 파이썬 환경세팅
2. docker compose에서 자신의 db 관련 설정과 마운트할 로컬 디렉토리 경로 세팅해주기
3. `docker-compose up`명령어를 통해서 한번에 빌드 및 컨테이너 실행
4. postgres의 컨테이너에 먼저 접속하여 'var/lib/postgresql/data/pg_hba.conf`에서 외부접속 허용, trust를 scram-sha-256으로 변경
5. redhat 컨테이너를 실행하고 해당 컨테이너에서 아래의 두 명령어를 통해 컨테이너들이 잘 연결되었는지 확인
```bash
psql -h postgres -U {DBUSER}
redis-cli -h redis
```
6. 로컬에서 `localhost:8080/pgadmin4`가 제대로 접속되는지 확인
7. 마지막으로 redhat서버 컨테이너에 디렉토리가 잘 마운트 되었는지 확인


