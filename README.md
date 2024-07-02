# Quick Start
1. Copy Linux folder and Upload to server
2. Change dir name
3. `docker-compose -f docker-compose_{name}.yml up --build`
4. [check](#check)

# Necessary Setting
// not for Quick Start User
1. Copy Linux folder
2. Upload Linux folder on Server and change directory name
3. Enter changed directory
4. Edit Dokerfile
    - Change SSH port number
    ```
    { bch:22, shw:23, ksh: 24}
    ```
5. Edit docker-compose.yaml
    - Change all container_name >> [default: redhat_bch, postgres_bch, redis_bch, pgadmin4_bch, zookeeper_bch, kafka_bch] 
    - Change "redhat" volumes to personal project folder <br>`[default: /home/bch/Project/ai_source(original):/mnt] -> [yours: {personal_project_folder}:/mnt]`
    - Change front port number
        ```bash
        bch: [redhat=22,redhat=9022,postgres=5343,redis=6380,pgadmin4=8080,zookeeper=2182,kafka=9093]
        shw: [redhat=23,redhat=9023,postgres=5344,redis=6381,pgadmin4=8081,zookeeper=2183,kafka=9094]
        ksh: [redhat=24,redhat=9025,postgres=5345,redis=6382,pgadmin4=8082,zookeeper=2184,kafka=9095]```
    - Change "kafka" environment
        - KAFKA_LISTENERS -> PLAINTEXT_HOST://0.0.0.0:{your_port}
        - KAFKA_ADVERTISED_LISTENERS -> PLAINTEXT_HOST://localhost:{your_port}

# docker command
```bash
cd {dir}
docker-compose -f custom-compose.yml up # 특정 yml파일을 이용하여 compose
docker-compose up         # docker-compose.yml파일 compose
docker-compose up --build # 여러개의 docker을 이미지 build 하면서 컨테이너 올림(최초실행시만)
docker-compose up         # 여러개의 docker 컨테이너 올림
docker-compose down       # 컨테이너 중단 및 제거 
docker-compose stop       # 실행중인 컨테이너 중단
docker-compose start      # 중단된 컨테이너 시작
docker-compose restart    # 모든 컨테이너 재기동동
docker-compose down -v    # 올려진 컨테이너 내리면서 볼륨 삭제
docker ps                 # 실행중인 컨테이너 리스트
docker ps -a              # 종료된 컨테이너까지 모두 리스트

docker start {container_name1} {container_name2} ..
docker stop {container_name or id} # 특정 컨테이너 종료
docker stop $(docker ps -a -q) # 실행중인 모든 컨테이너 종료

docker rm {container_name or id} # 특정 컨테이너 제거
docker rm $(docker ps -aq)  # 실행중인 모든 컨테이너 제거

docker rmi {image_name or id} # 특정 이미지 제거
docker rmi $(docker images -q) # 존재하는 모든 이미지 제거

docker volume ls # 볼륨 목록 확인인
docker volume prune # 사용하지않는 모든 볼륨 제거
docker volume rm $(docker volumne ls -q) # 모든 볼륨 제거
docker volume rm {volume_name} # 특정 볼륨 제거거

docker exec -it {container_name} /bin/bash # postgresql 컨테이너는 /bin/sh
```

# Utils
- Vscode: Dev Containers

# Check
- Postgres <br>
 : redhat container bash에서 `psql -h postgres -U mdaai -d mdadb2`, pw=`mda8932!` 접속 확인후 `\q`로 종료
- redhat <br>
 : redhat container bash에서서 `redis-cli -h redis`로 접속확인 후 `exit`로 종료
- pgadmin4 <br>
 : 주소창에 `118.38.20.101:{pgadmin4_port}`에 접속 <br>
 : pgadmin4 `id:gcai123@gmail.com` / `pw:gcai123!` 입력하여 접속 <br>
 : 서버 등록시 hostip=`postgres`, maindb=`mdadb2`, user=`mdaai`, pw=`mda8932!`  
- mount <br>
 : redhat container `/mnt`에 자신이 마운트한 프로젝트 폴더가 있는지 확인
- api test
 : 로컬 chrome에서 `118.38.20.101:{redhat_port2}/docs` 로 접속하여 fastapi docs 페이지 확인인 
