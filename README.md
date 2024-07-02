# Quick Start
1. Copy `Linux/{USERNAME}_docker` folder and Upload to server
2. `docker-compose -f docker-compose_{name}.yml up`
3. [check](#check)

# Necessary Setting
// not for Quick Start User
1. Copy `Linux/{USERNAME}_docker` folder and Upload to server
2. Enter changed directory
3. Edit Dokerfile
    - Change SSH port number
    ```
    { bch:22, shw:23, ksh: 24}
    ```
4. Edit docker-compose.yaml
    - Change all container_name >> [default: redhat_bch, postgres_bch, redis_bch, pgadmin4_bch, zookeeper_bch, kafka_bch] 
    - Change "redhat" volumes to personal project folder <br>`[default: /home/bch/Project/ai_source(original):/mnt] -> [yours: {personal_project_folder}:/mnt]`
    - Change front port number
        ```bash
        bch: [redhat=`22`,redhat=`9022`,postgres=`5343`,redis=`6380`,pgadmin4=`8080`,zookeeper=`2182`,kafka=`9093`]
        shw: [redhat=`23`,redhat=`9023`,postgres=`5344`,redis=`6381`,pgadmin4=`8081`,zookeeper=`2183`,kafka=`9094`]
        ksh: [redhat=`24`,redhat=`9025`,postgres=`5345`,redis=`6382`,pgadmin4=`8082`,zookeeper=`2184`,kafka=`9095`]```
    - Change "kafka" environment
        - KAFKA_LISTENERS -> PLAINTEXT_HOST://0.0.0.0:{your_port}
        - KAFKA_ADVERTISED_LISTENERS -> PLAINTEXT_HOST://localhost:{your_port}
# Useful Docker command
```bash
cd {USERNAME}_docker
docker-compose -f {composefilename}.yml up --build # 처음 실행시 image build 및 컨테이너 실행(bch유저만 --build옵션 추가. 나머진 X)
docker ps -a                                       # 종료포함 모든 컨테이너 리스트
docker stop {cotainer_id1} {cotainer_id2} ..       # 특정 컨테이너 종료
docker rm {cotainer_id1} {cotainer_id2} ..         # 특정 컨테이너 삭제
docker images                                      # 모든 이미지 리스트
docker rmi {image_id1} {image_id2}                 # 특정 이미지 삭제
docker volume prune                                # 컨테이너에서 사용중이지 않은 volumne 삭제제
```

# ALL Docker command
```bash
cd {dir}
docker-compose -f docker_compose_{USERNAME}.yml up # 특정 yml파일을 이용하여 컨테이너 생성
docker-compose up         # docker-compose.yml파일 이용하여 컨테이너 생성 및 실행
docker-compose up --build # 처음 실행시 image build 및 컨테이너 실행
docker-compose up         # 여러개의 docker 컨테이너 올림
docker-compose down       # 모든 존재하는 컨테이너 중단 및 제거(주의)
docker-compose stop       # 모든 실행중인 컨테이너 중단(주의)
docker-compose start      # 중단된 컨테이너 시작
docker-compose restart    # 모든 컨테이너 재기동
docker-compose down -v    # 모든 실행중인 컨테이너 삭제 및 볼륨 삭제
docker ps                 # 실행중인 컨테이너 리스트
docker ps -a              # 종료포함 모든 컨테이너 리스트
docker images             # 모든 이미지 리스트

docker start {container_name1} {container_name2} .. # 특정 컨테이너 시작작
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
 : 로컬 chrome에서 `118.38.20.101:{redhat_port2}/docs` 로 접속하여 fastapi docs 페이지 확인

 # Changelog

 - 2024/07/02
 : DELETE personal Dockfile(`Dockerfile.bch`,`Dockerfile.shw`,`Dockerfile.ksh`)
   >> redhat image 생성의 중복을 막기 위해서(개당 8GB)
 : CHANGE Dockerfile contents 
   >> remove ssh connection & add CMD command (불필요한 ssh 삭제 및 컨테이너 유지)
 : CHANGE redhat image_name
   >> redhat 이미지 이름을 gc_redhat으로 지정하여 하나의 이미지만 유지하도록 설정
  summary: 중복사용되는 redhat image 파일 생성을 방지하기 위한 commit. (결과적으로 16GB 보존됨)   

 - 2024/07/02(2)
 : CREATE personal directory(`bch_docker`,`ksh_docker`,`shw_docker`)
   >> 개인 폴더만 복사하여 서버에 업로드하고 폴더 내에서 실행하면 됨.
 : DELETE Dockerfile
   >> bch 유저만 최초실행시 Dockerfile로 `gc_redhat`이미지 생성하고, 나머지는 compose로 image 받아오기만 함
  
