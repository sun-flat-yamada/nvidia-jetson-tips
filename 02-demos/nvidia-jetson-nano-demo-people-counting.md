# Jetson nano DEMO - People Counting

人間がある境界線を越えた時の方向でカウントするDEMO

- GitHub [JardinRyu
/
Jetson_Nano_People_Counting
](https://github.com/JardinRyu/Jetson_Nano_People_Counting)


## How to setup

1. JetPack 4.5.1 にする (see. [01-upgrade/how-to-upgrade-jetpack.md](../01-upgrade/how-to-upgrade-jetpack.md))
2. 上記リポジトリの手順に従いInstallして実行する

```sh:setup
mkdir -p ~/workspace/demo/people-counting
cd ~/workspace/demo/people-counting/

git clone https://github.com/JardinRyu/Jetson_Nano_People_Counting
cd Jetson_Nano_People_Counting/

./install.sh

cd ssd/

./install.sh
./build_engines.sh
```

USBカメラの場合

```sh:usage
# CameraID = 0
pytyhon3 usbcam_tracking.py
```

MIPI Cameraの場合 (ex. Raspi Cam v2)

```sh:usage
python3 mipicam_tracking.py
```
