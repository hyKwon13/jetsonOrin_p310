```bash

# python3.10 설치
sudo apt update
sudo apt install -y build-essential libssl-dev zlib1g-dev \
libncurses5-dev libncursesw5-dev libreadline-dev libsqlite3-dev \
libgdbm-dev libdb5.3-dev libbz2-dev libexpat1-dev liblzma-dev \
libffi-dev uuid-dev

cd /usr/src
sudo wget https://www.python.org/ftp/python/3.10.0/Python-3.10.0.tgz
sudo tar xzf Python-3.10.0.tgz

cd Python-3.10.0
sudo ./configure --enable-optimizations --enable-shared
sudo make altinstall
sudo ldconfig

sudo apt update
sudo apt install python3-pip
python3.10 -m pip install --upgrade pip

# numpy, opencv 설치
python3.10 -m pip install numpy
python3.10 -m pip install opencv-python
sudo apt install nvidia-opencv-dev nvidia-opencv

# 각종 패키지 설치
python3.10 -m pip install --user [패키지이름]

# mysql 설치
sudo apt install mysql-server

# pytorch - jetson 설치
https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048
에서 whl 파일 받아서 설치. python3.10, cuda12.4로 설치
pip install torch-2.3.0-cp310-cp310-linux_aarch64.whl
pip install torchvision-0.18.0a0+6043bc2-cp310-cp310-linux_aarch64.whl

# GPIO 설치
python3.10 -m pip install Jetson.GPIO

# (옵션) jtop 설치
sudo pip3 install -U jetson-stats
```
