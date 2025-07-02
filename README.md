```bash
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
python3.10 -m pip install numpy

python3.10 -m pip install opencv-python
```
