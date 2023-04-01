# how-to-use-ufw

Ubuntu の Firewallツール (UFW:Uncomplicated firewall)で試してみる。

※ [Wikipedia](https://ja.wikipedia.org/wiki/Uncomplicated_Firewall) から引用
> Uncomplicated Firewall（UFW）は、簡単に使用できるように設計された、netfilter（英語版）ファイアウォールの管理プログラムである。少数のシンプルなコマンドからなるコマンドラインインターフェイスを使用する。内部の設定には、iptablesを使用している。UFWは、8.04 LTS以降のすべてのUbuntuインストールでデフォルトで使用可能である。

---

## はじめに

### 試行環境

今回試した環境は以下（※一応記録しておく）

```
cat /etc/os-release
```
実行結果
```
NAME="Ubuntu"
VERSION="20.04.5 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04.5 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
```

### 初期状態

初期状態はインアクティブになっている。

```
systemctl status ufw
```
実行結果
```
● ufw.service - Uncomplicated firewall
     Loaded: loaded (/lib/systemd/system/ufw.service; enabled; vendor preset: enabled)
     Active: inactive (dead)
       Docs: man:ufw(8)
```


## FW設定変更

### TCP接続を許可する

SSH通信用にTCP接続を許可する。

```
ufw allow 22/tcp
```
実行結果
```
Rules updated
Rules updated (v6)
```

### 接続を許可する

任意の通信用にTCP/UDP接続を許可する。

```
ufw allow 80
```
実行結果
```
Rules updated
Rules updated (v6)
```

### 特定IPからの接続を許可する

特定IP（135.22.65.0/24）から9090ポートへのTCP通信を全て許可する。

```
ufw allow from 135.22.65.0/24 to any port 9090 proto tcp
```
実行結果
```
Rules updated
```

特定IP（135.22.65.0/24）から9091ポートへのTCP通信を全て許可する。

```
ufw allow from 135.22.65.0/24 to any port 9091 proto tcp
```
実行結果
```
Rules updated
```


## FW有効化

ファイアウォールを有効にして、システム起動時に自動実行されるようにする。

```
ufw enable
```
実行結果
```
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
```
ファイアウォールはアクティブかつシステムの起動時に有効化されます。


## FW実行／停止

### 手動で実行する

```
systemctl start ufw
```
特に何も表示されないため、以下のコマンドでステータスを確認する。
```
systemctl status ufw
```

実行結果
```
● ufw.service - Uncomplicated firewall
     Loaded: loaded (/lib/systemd/system/ufw.service; enabled; vendor preset: enabled)
     Active: active (exited) since Fri 2023-03-31 22:14:06 EDT; 2s ago
       Docs: man:ufw(8)
    Process: 21145 ExecStart=/lib/ufw/ufw-init start quiet (code=exited, status=0/SUCCESS)
   Main PID: 21145 (code=exited, status=0/SUCCESS)

Mar 31 22:14:06 node01 ufw-init[21149]: Firewall already started, use 'force-reload'
```

### 手動で停止する

```
systemctl stop ufw
```
特に何も表示されないため、以下のコマンドでステータスを確認する。
```
systemctl status ufw
```

実行結果
```
● ufw.service - Uncomplicated firewall
     Loaded: loaded (/lib/systemd/system/ufw.service; enabled; vendor preset: enabled)
     Active: inactive (dead) since Fri 2023-03-31 22:19:12 EDT; 2s ago
       Docs: man:ufw(8)
    Process: 21145 ExecStart=/lib/ufw/ufw-init start quiet (code=exited, status=0/SUCCESS)
    Process: 21953 ExecStop=/lib/ufw/ufw-init stop (code=exited, status=0/SUCCESS)
   Main PID: 21145 (code=exited, status=0/SUCCESS)

Mar 31 22:14:06 node01 ufw-init[21149]: Firewall already started, use 'force-reload'
```

---

## 参考
* https://ja.wikipedia.org/wiki/Uncomplicated_Firewall
* https://qiita.com/RyoMa_0923/items/681f86196997bea236f0
* https://minory.org/ubuntu-ufw.html
* https://server-network-note.net/2022/07/ubuntu-server-22-04-lts-firewall-ufw/
