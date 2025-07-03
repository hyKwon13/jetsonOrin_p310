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
python3.10 -m pip install --user "numpy<2"
python3.10 -m pip install opencv-python
sudo apt install nvidia-opencv-dev nvidia-opencv
find /usr -name "cv2*.so" 이렇게해서 나온
/usr/lib/python3.10/dist-packages/cv2/python-3.10/cv2.cpython-310-aarch64-linux-gnu.so
이 경로의 /usr/lib/python3.10/dist-packages 이 부분을 넣어서 환경변수 설정
export PYTHONPATH=/usr/lib/python3.10/dist-packages/:$PYTHONPATH
 
.bashrc에 아래 내용 추가
echo 'export PYTHONPATH=/usr/lib/python3.10/dist-packages:$PYTHONPATH' >> ~/.bashrc
source ~/.bashrc




# 각종 패키지 설치
python3.10 -m pip install --user [패키지이름]

# mysql 설치
sudo apt install mysql-server

# mysql 진입
sudo mysql

# 비밀번호 설정
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'hynux1357';
FLUSH PRIVILEGES;
 
CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'hynux1357';
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;

# 스키마 생성
CREATE DATABASE hydb;

# pytorch - jetson 설치
https://forums.developer.nvidia.com/t/pytorch-for-jetson/72048
에서 whl 파일 받아서 설치. python3.10, cuda12.4로 설치
pip install torch-2.3.0-cp310-cp310-linux_aarch64.whl
pip install torchvision-0.18.0a0+6043bc2-cp310-cp310-linux_aarch64.whl

# GPIO 설치
python3.10 -m pip install Jetson.GPIO

# V4L2 설치
sudo apt install v4l-utils

# (옵션) jtop, nano 설치
sudo pip3 install -U jetson-stats
sudo apt install nano
```
