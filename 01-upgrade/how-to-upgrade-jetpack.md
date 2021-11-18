# How to upgrade JetPack - JetPack を Upgrade する

@2021-11-18 JetPack 4.6 の時点において

以下2つのパターンで方法が違う。

- 新しいポイントリリースにUpgradeする場合 (ex. 4.5 -> 4.5.1)
- 新しいマイナーリリースにUpgradeする場合 (ex. 4.4 -> 4.5)

see

- JetsonでのJetPack Upgrade方法
  - [Updating a Jetson Device](https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/updating_jetson_and_host.html#wwpID0E0AL0HA)
- JetPackのVersionとL4TのVersion対応関係一覧
  - [JetPack Archive](https://developer.nvidia.com/embedded/jetpack-archive)


## 新しいポイントリリースにUpgradeする場合 - To update to a new point release

```
sudo apt update
apt list --upgradable
sudo apt upgrade
sudo reboot now
```


## 新しいマイナーリリースにUpgradeする場合 - To update to a new minor release


```
sudo vi /etc/apt/sources.list.d/nvidia-l4t-apt-source.list
```

以下のように編集する。

```
deb https://repo.download.nvidia.com/jetson/common <release> main
deb https://repo.download.nvidia.com/jetson/<platform> <release> main
```

Jetson nano で JetPack4.5.1 にしたい場合は、L4T 32.5.1 なので、以下のようにする。

- <platform> = t210
- <release> = 32.5

release マイナーバージョンまでの指定で、ポイントリリースの番号(2つ目のピリオド以降)は指定しない方式となっているので注意。32.5.1と指定すると、`sudo apt update`した時点でそんなリポジトリは存在しないというエラーになる。

具体的には以下。

```
deb https://repo.download.nvidia.com/jetson/common 32.5 main
deb https://repo.download.nvidia.com/jetson/t210 32.5 main
```

編集したら以下のコマンドでupgradeする。
40分以上かかるだろう。

```
sync
sudo apt update
sudo apt dist-upgrade -y
```


-----

EOF
