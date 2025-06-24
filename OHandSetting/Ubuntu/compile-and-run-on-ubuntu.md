# Compile and Run on Ubuntu

## Install peak-linux-driver

peak-linux-driver need GCC 12 or later.
For GCC version < 12, you can install gcc-12 using the following commands:

```bash
sudo apt-get update
sudo apt-get install gcc-12
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-12 12
sudo apt install libpopt-dev
```

```bash
wget https://www.peak-system.com/fileadmin/media/linux/peak-linux-driver-8.20.0.tar.gz
tar -zxvf peak-linux-driver-8.20.0.tar.gz
cd peak-linux-driver-8.20.0
make && sudo make install
```

## Install essential libs for QT6

```bash
sudo apt install build-essential
sudo apt install libgl1-mesa-dev libglvnd-dev -y
sudo apt install qt6-base-dev qt6-tools-dev-tools -y
sudo apt install python-is-python3
```
