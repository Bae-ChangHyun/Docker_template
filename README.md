# [Docker Engine Install](https://docs.docker.com/engine/install/ubuntu/)

## Ubuntu Install
```bash
# 충돌이 발생할 수 있는 기존 패키지들 제거
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo service docker start
```

# GPU
docker container 내부에서 GPU를 사용하기 위해서는 설치해야함.
```
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list

sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-toolkit.list

sudo apt-get update

sudo apt-get install -y nvidia-container-toolkit

sudo nvidia-ctk runtime configure --runtime=docker

sudo systemctl restart docker
```

## Tip
1. Docker 설치 후,아래와 같이 이미지를 pull 하려는데 오류가 발생할 경우, 권한을 부여하면 됨. >> [Reference](https://docs.docker.com/engine/install/linux-postinstall/)
``docker.errors.DockerException: Error while fetching server API version: ('Connection aborted.', PermissionError(13, 'Permission denied'))[105018] Failed to execute script docker-compose``
```bash
sudo groupadd docker
sudo usermod -aG docker $USER
newgrp docker 
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R
```
2. Vscode 등과 같이 docker 컨테이너를 보려고 하는데, 오류가 뜰 경우 docker socket에 권한을 부여하면 됨.>>[Reference](https://github.com/occidere/TIL/issues/116)
```bash
sudo chmod 666 /var/run/docker.sock
```
