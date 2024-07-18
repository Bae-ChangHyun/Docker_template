# For server user

## Quick Start
1. Clone repository to Server
2. `cd /docker_setting/Server/{USERNAME}_docker`
3. `docker-compose -f docker-compose_{USERNAME}.yml up`
4. [check](#check)

## Necessary Setting(Not for Quick Start User)
새로운 USER이 추가되었을 때는 아래의 설정 방법을 따라서 docker-compose 파일을 생성하면 됩니다.
1. <b>Copy all files in `docker_setting/Server/sample_docker`</b>
<br>
2. <b>Edit docker-compose_sample.yaml</b>
    <b>2-1. Change all container name</b>
        컨테이너 이름은 무엇으로 짓든 상관없으나, 혼동의 여지가 있기 때문에 알기 쉽게 설정
        [ex: redhat_bch, postgres_bch, redis_bch, pgadmin4_bch, zookeeper_bch, kafka_bch] 
    <b>2-2. Change "redhat" volumes to personal project folder</b> 
         컨테이너의 mount volume 경로를 자신의 경로로 바꿔줘야 함.{MYDIR}:{DOCKE_DIR}으로 작성하면 로컬의 MY_DIR이 컨테이너 내부의 DOCKER_DIR과 매핑됨. 
         [ex: /home:/mnt] = 로컬의 home과 컨테이너 내부의 mnt가 동일한 디렉토리를 가리킴
    <b>2-3.Change front port number</b>
         하나의 서버에 컨테이너들을 올려서 사용하는 것이기 때문에 포트가 같으면 안됨. {외부포트}:{내부포트}
         컨테이너는 독립적이기 때문에 앞의 도커내부에서의 포트를 나타내는 뒷부분은 변경할 필요없고 앞부분만 변경해주면 됨.
         아래는 이미 사용중인 외부(서버)포트
        ``` bash
        bch: [redhat=`9022`,postgres=`5343`,redis=`6380`,pgadmin4=`8080`,zookeeper=`2182`,kafka=`9093`]
        shw: [redhat=`9023`,postgres=`5344`,redis=`6381`,pgadmin4=`8081`,zookeeper=`2183`,kafka=`9094`]
        ksh: [redhat=`9024`,postgres=`5345`,redis=`6382`,pgadmin4=`8082`,zookeeper=`2184`,kafka=`9095`]```
    <b>2-4.Change "kafka" environment</b>
          Kafka 이미지에 설정해둔 아래의 포트들도 위에서 변경한 kafka 포트에 맞게 변경해줘야 함.
        - KAFKA_LISTENERS -> PLAINTEXT_HOST://0.0.0.0:{your_port}
        - KAFKA_ADVERTISED_LISTENERS -> PLAINTEXT_HOST://localhost:{your_port}

## Useful Docker command
```bash
cd {USERNAME}_docker
docker-compose -f {composefilename}.yml up --build # docker-file의 이미지를 빌드하면서 컨테이너 올림.
docker ps -a                                       # 종료된 컨테이너 포함힌 모든 컨테이너 리스트
docker stop {cotainer_id1} {cotainer_id2} ..       # 특정 컨테이너 종료
docker rm {cotainer_id1} {cotainer_id2} ..         # 특정 컨테이너 삭제
docker images                                      # 모든 이미지 리스트
docker rmi {image_id1} {image_id2}                 # 특정 이미지 삭제
docker volume prune                                # 컨테이너에서 사용중이지 않은 volume 삭제
docker cp {LOCAL_PATH} {CONATINER_DIR}             # 로컬의 파일을 컨테이너로 복사
docker inspect {container_id}                      # 컨테이너 정보 확인
```

## ALL Docker command
```bash
cd {dir}
docker-compose up                                  # docker-compose.yml파일 이용하여 컨테이너 생성 및 실행
docker-compose -f docker_compose_{USERNAME}.yml up # 특정 yml파일을 이용하여 컨테이너 생성
docker-compose up --build                          # 처음 실행시 image build 및 컨테이너 실행
docker-compose up                                  # 여러개의 docker 컨테이너 올림
docker-compose down                                # 모든 존재하는 컨테이너 중단 및 제거(주의)
docker-compose stop                                # 모든 실행중인 컨테이너 중단(주의)
docker-compose start                               # 중단된 컨테이너 시작
docker-compose restart                             # 모든 컨테이너 재기동
docker-compose down -v                             # 모든 실행중인 컨테이너 삭제 및 볼륨 삭제
docker ps                                          # 실행중인 컨테이너 리스트
docker ps -a                                       # 종료된 컨테이너 포함힌 모든 컨테이너 리스트
docker images                                      # 모든 이미지 리스트
docker cp {LOCAL_PATH} {CONATINER_DIR}             # 로컬의 파일을 컨테이너로 복사

docker start {container_name1} {container_name2} . # 특정 컨테이너 시작
docker stop {container_name or id}                 # 특정 컨테이너 종료
docker stop $(docker ps -a -q)                     # 실행중인 모든 컨테이너 종료

docker rm {container_name or id}                   # 특정 컨테이너 제거
docker rm $(docker ps -aq)                         # 실행중인 모든 컨테이너 제거

docker rmi {image_name or id}                      # 특정 이미지 제거
docker rmi $(docker images -q)                     # 존재하는 모든 이미지 제거

docker volume ls                                  # 볼륨 목록 확인
docker volume prune                               # 사용하지 않는 모든 볼륨 제거
docker volume rm $(docker volumne ls -q)          # 모든 볼륨 제거
docker volume rm {volume_name}                    # 특정 볼륨 제거

docker exec -it {container_name} /bin/bash        # 컨테이너 터미널에 접속(!postgresql 컨테이너는 /bin/sh)
```

## Utils
- Vscode: Dev Containers

## Check
위의 설정대로 컨테이너들이 올라간 것을 확인했다면 아래 명령어들을 통해 컨테이너들이 정상적으로 동작하는지 확인할 것.
- Postgres <br>
 : redhat container bash에서 `psql -h postgres -U mdaai -d mdadb2`, pw=`mda8932!` 접속 확인후 `\q`로 종료
- redhat <br>
 : redhat container bash에서서 `redis-cli -h redis`로 접속확인 후 `exit`로 종료
- pgadmin4 <br>
 : 주소창에 `118.38.20.101:8080`에 접속 <br>
 (!8080이 안열려있다면, 8081 혹은 8082 --> 같은 db를 공유중이라 여러 포트에서 동시접속 불가)
 : pgadmin4 `id:gcai001127@gmail.com` / `pw:gcai123!` 입력하여 접속 <br>
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

 - 2024/07/18
 : Delete Unnecessary command
   >> 불필요한 명령어들을 제거 및 수정(ssh관련 삭제, 오류 제거, requirements.txt 개인폴더로 추가)
  
