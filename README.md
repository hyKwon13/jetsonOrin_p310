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

# GPIO 설정
 sudo /opt/nvidia/jetson-io/jetson-io.py -> Configure Jetson 40pin Header -> Configure header pins manuallly
```


![capture1](https://github.com/hyKwon13/jetsonOrin_p310/raw/main/capture1.PNG)

```bash
# V4L2 설치
sudo apt install v4l-utils

# pycuda 설치
sudo apt install -y build-essential python3.10-dev \
                    libpython3.10-dev pkg-config git \
                    nvidia-cuda-dev

echo 'export PATH=/usr/local/cuda/bin:$PATH'           >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
echo 'export CPATH=/usr/local/cuda/include:$CPATH'     >> ~/.bashrc
source ~/.bashrc

git clone https://github.com/inducer/pycuda.git
cd pycuda
python3.10 -m pip install --user .

# (옵션) jtop, nano 설치
sudo pip3 install -U jetson-stats
sudo apt install nano
```


# Vision Sensor Server systemd Service Setup

## 1. 서비스 유닛 파일 만들기

```bash
sudo nano /etc/systemd/system/vision-server.service
```

아래 내용을 붙여넣어:

```ini
[Unit]
Description=Vision Sensor Server
After=network.target

[Service]
WorkingDirectory=/home/visionsensor/vision
ExecStart=/usr/bin/python3.10 /home/visionsensor/vision/server.py
Restart=always
RestartSec=5
User=visionsensor

[Install]
WantedBy=multi-user.target
```

> ⚠️ **주의**  
> * `ExecStart` 경로는 `which python3.10` 명령으로 확인한 정확한 Python 실행 파일 위치로 변경하세요.  
> * `User` 값은 서비스를 실행할 실제 사용자 계정(예: `visionsensor`)인지 확인하세요.

## 2. 서비스 등록 및 자동 시작 설정

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable vision-server.service
sudo systemctl start vision-server.service
```

## 3. 서비스 상태 확인 및 로그 보기

```bash
# 현재 상태 확인
sudo systemctl status vision-server.service

# 실시간 로그 출력
journalctl -u vision-server.service -f
```

