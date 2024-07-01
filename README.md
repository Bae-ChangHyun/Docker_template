# Quick Start
- 1. Copy Linux folder and Upload to server
- 2. Change dir name
- 3. `docker-compose -f docker-compose_{name}.yml up --build`
- 4. [check](#check)

# Necessary Setting

- 1. Copy Linux folder
- 2. Upload Linux folder on Server and change directory name
- 3. Enter changed directory
- 4. Edit Dokerfile
    - 4-1. Change SSH port number{
        // already used 
        bch:22, shw:23, ksh: 24}
- 5. Edit docker-compose.yaml
    - 5-1. Change all container_name >> [default: redhat_bch, postgres_bch, redis_bch, pgadmin4_bch, zookeeper_bch, kafka_bch] 
    - 5.2. Change "redhat" volumes to personal project folder >> [default: /home/bch/Project/ai_source(original):/mnt] -> [yours: {personal_project_folder}:/mnt] 
    - 5-3. Change front port number{
        // already used
        bch: [redhat=22,redhat=9022,postgres=5343,redis=6380,pgadmin4=8080,zookeeper=2182,kafka=9093]
        shw: [redhat=23,redhat=9023,postgres=5344,redis=6381,pgadmin4=8081,zookeeper=2183,kafka=9094]
        ksh: [redhat=24,redhat=9024,postgres=5345,redis=6382,pgadmin4=8082,zookeeper=2184,kafka=9095]
    }
    - 5-4. Change "kafka" environment
        - 5-4-1. KAFKA_LISTENERS -> PLAINTEXT_HOST://0.0.0.0:{your_port}
        - 5-4-2. KAFKA_ADVERTISED_LISTENERS -> PLAINTEXT_HOST://localhost:{your_port}

# command
```bash
cd {dir}
docker-compose -f custom-compose.yml up
docker-compose up --build # 여러개의 docker을 이미지 build 하면서 컨테이너 올림(최초실행시만)
docker-compose up         # 여러개의 docker 컨테이너 올림
docker-compose down       # 올린 컨테이너 내림
docker-compose down -v    # 올려진 컨테이너 내리면서 볼륨 삭제
docker ps                  # 전체 실행중인 컨테이너 리스트
docker ps -a              # 종료된 컨테이너까지 모두 리스트
docker stop $(docker ps -a -q) # 컨테이너 모두 종료
docker rm $(docker ps -aq) 
docker rmi $(docker images -q)
docker volume rm $(docker volumne ls -q)
docker exec -it {container_name} /bin/bash # postgresql 컨테이너는 /bin/sh
```

# Utils
- Vscode: Dev Containers

# Check
- Postgres
 : redhat 컨테이너에 접속하여 터미널을 열고 `psql -h postgres -U mdaai -d mdadb2` 입력후 pw=`mda8932!`입력하여 접속되는지 확인후 `\q`로 종료
- redhat
 : redhat 컨테이너에 접속하여 터미널을 열고 `redis-cli -h redis`입력하여 접속확인후 `exit`로 종료
- pgadmin4
 : chrome을 열고 `118.38.20.101:{pgadmin4_port}`에 접속
 : pgadmin4 `id:gcai123@gmail.com` / `pw:gcai123!` 입력하여 접속
 : 서버 등록시 hostip=`postgres`, maindb=`mdadb2`, user=`mdaai`, pw=`mda8932!`  
- mount
 : redhat 컨테이너 `/mnt`경로에 자신이 마운트한 프로젝트 폴더에 정상적으로 접속되는지 확인
