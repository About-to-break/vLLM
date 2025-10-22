# Контейнер с LLM
## ⚙️ Cистемные требования
- Выпуск ОС поддерживающий CUDA 12.8 -> Ubuntu 22.04+
- GPU поддерживающий CUDA 12.8 -> RTX 3000+
## Развернуть на Ubuntu 22.04+
1. Установка Docker
```bash
sudo apt update && sudo apt upgrade -y

sudo apt install ca-certificates curl gnupg lsb-release -y

sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
```
Проверка
```bash
sudo docker run hello-world
```
2. Установка зависимостей Nvidia
```bash
sudo add-apt-repository ppa:graphics-drivers/ppa -y

sudo apt install nvidia-driver-575 -y # RTX 3000
sudo apt install nvidia-driver-570-open -y #RTX 5000

sudo reboot
nvidia-smi

curl -s -L https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg


echo "deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://nvidia.github.io/libnvidia-container/stable/deb/amd64   /" | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
sudo apt update
sudo apt install -y nvidia-container-toolkit

sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker
```
Проверка
```bash
docker run --rm --gpus all nvidia/cuda:12.3.1-base-ubuntu22.04 nvidia-smi
```
---

