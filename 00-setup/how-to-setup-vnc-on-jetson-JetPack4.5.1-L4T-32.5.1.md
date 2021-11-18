# How to setup VNC on Jetson - Jetson nano を VNC でリモートデスクトップ接続できるようにする @ JetPack4.5.1 L4T 32.5.1

Jetson nano にリモートデスクトップ接続できるようにする方法をまとめます。
これで毎回HDMI接続して表示確認しなくてよくなります。

see

- (Setup VNC)[https://developer.nvidia.com/embedded/learn/tutorials/vnc-setup]


## Prerequisites

Jetson nano (ver B01)
JetPackのVersionなどは以下

```sh
nv@nv-jt:~/repos/jetsonUtilities$ python jetsonInfo.py
NVIDIA Jetson Nano (Developer Kit Version)
 L4T 32.5.1 [ JetPack 4.5.1 ]
   Ubuntu 18.04.5 LTS
   Kernel Version: 4.9.201-tegra
 CUDA 10.2.89
   CUDA Architecture: 5.3
 OpenCV version: 4.1.1
   OpenCV Cuda: NO
 CUDNN: 8.0.0.180
 TensorRT: 7.1.3.0
 Vision Works: 1.6.0.501
 VPI: ii libnvvpi1 1.0.15 arm64 NVIDIA Vision Programming Interface library
 Vulcan: 1.2.70
```

jetsonInfo.py の Install 方法は後述。


## jetsonInfo.py のInstall方法

```bash
git clone https://github.com/jetsonhacks/jetsonUtilities
cd jetsonUtilities/
python jetsonInfo.py
```


## Desktop Sharing を有効化する

JetPack4.3 の SD Card Image を使った場合、Desktop Sharingの設定を開くとCrashする。
既知の問題であるので、いくつか設定を行えば回避できる。
新しいVersionだと元から対策されているかもしれない。

基本的な手順は以下サイトを参照。

- https://qiita.com/iwatake2222/items/a3bd8d0527dec431ef0f
- https://www.hackster.io/news/getting-started-with-the-nvidia-jetson-nano-developer-kit-43aa7c298797


具体的な要点を順にまとめる。

まず、Vino (Desktop Sharing を行うVNC Server) の設定を更新する。
対象の以下設定ファイルを開いて更新する。

```
sudo vi /usr/share/glib-2.0/schemas/org.gnome.Vino.gschema.xml
```

末尾のkey要素のレベルに以下を追加する。

```
    <key name='enabled' type='b'>
      <summary>Enable remote access to the desktop</summary>
        <description>
          If true, allows remote access to the desktop via the RFB
          protocol. Users on remote machines may then connect to the
          desktop using a VNC viewer.
        </description>
      <default>false</default>
    </key>
```

次に以下を実行して更新した設定を反映する。

```
sudo glib-compile-schemas /usr/share/glib-2.0/schemas
```

これを行えばDesktop Sharingの設定がCrashせずに開けるようになる。

Jetson nanoにログイン(GUIで)して、All Settings -> Desktop Sharing を開く。
上記カスタムをするまでは開いたタイミングでCrashする。

この設定画面でAllow other users to view your desktop を有効にする。

次に、デスクトップ左上のDashアイコンをクリックして
`Startup Application Preferences`とタイプして設定を開く

Add をクリックして以下のエントリーを追加する。

Name    : vino
Command : /usr/lib/vino/vino-server
Comment : VNC Server

最後に、暗号化を無効にする。
Vinoでサポートされている暗号化はTLSセキュリティタイプ(18)であり、これは殆どのVNC Clientにサポートされていない。
この互換性の問題は５年以上前から存在しているので、すぐに解決される期待は薄い。

```
gsettings set org.gnome.Vino require-encryption false
gsettings set org.gnome.Vino prompt-enabled false
```

HDMI接続していない場合、画面解像度は600x480がデフォルトになっていて小さい。
大きくしたい場合、以下を編集する。

```
sudo vi /etc/X11/xorg.conf
```

以下を最後に追加

```
Section "Monitor"
   Identifier "DSI-0"
   Option    "Ignore"
EndSection

Section "Screen"
   Identifier    "Default Screen"
   Monitor        "Configured Monitor"
   Device        "Default Device"
   SubSection "Display"
       Depth    24
       Virtual 1280 800
   EndSubSection
EndSection
```

see
https://forums.developer.nvidia.com/t/jetson-tx1-desktop-sharing-resolution-problem-without-real-monitor/48041/11


ここまでやったらJetsonを再起動する。
再起動後、Host (Win10の開発マシン等) からVNC Clientで接続すればよい。
Tight VNCがおススメ。

