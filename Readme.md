# 各種規定値
|パラメータ|値|
|:-:|:-:|
|ユーザー名|user|
|パスワード|pass|
|コンテナ内IP|192.168.50.10|
|利用イメージ|ubuntu-minimal:j|

# 初期設定
### プロファイルの作成
```sh
lxc network create develop --type=bridge ipv4.address=192.168.50.1/24 ipv4.nat=true ipv6.address=none
lxc profile create develop-net
lxc profile device add develop-net eth0 nic network=develop
lxc profile device add develop-net root disk path=/ pool=pool
lxc profile create develop
lxc profile device add develop root disk path=/ pool=pool
```

### データ保存用のボリュームの作成
```sh
lxc storage volume create pool android
lxc storage volume create pool Program
lxc storage volume create pool Key
```

# android SDK関連
### android studioの起動
```sh
# userにログインして以下を実行
studio
# GUIで android SDK Location の設定とダウンロード
```
### flutterの設定
```sh
flutter doctor --android-licenses
```
### 各種PATH
- android studio  
  `/usr/local/src/android-studio/studio.sh`
- android SDK Location  
  `/home/android/Sdk`


# ポートフォワーディングの設定
```sh
HOST_IP=192.168.1.2
lxc network forward create develop $HOST_IP
lxc network forward port add develop $HOST_IP tcp 20022 192.168.50.10 22
```

# 接続コマンド
```sh
HOST_IP=192.168.1.2
ssh -XYp 20022 -i flutter_LXD/key/key user@$HOST_IP
```

# git用鍵ファイルの設定
```sh
HOST_IP=192.168.1.2
KEY_FILE=path/to/key
scp -P 10022 -i flutter_LXD/key/key $KEY_FILE user@$HOST_IP:/home/key/git
```

# USBデバイス
```sh
# デバイスの追加
lxc config device add flutter phone usb vendorid=<ID> mode=0666
# デバイスの削除
lxc config device remove flutter phone
```
