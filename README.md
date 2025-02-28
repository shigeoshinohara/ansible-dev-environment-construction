# Ansible確認用お試し環境作成手順

---
## 仮想マシンの作成
> VirtualBoxのインストール  

  [VirtualBoxダウンロードページ](https://www.virtualbox.org/wiki/Downloads)  
  ※インストールはデフォルトの状態で問題なし。
<br>
<br>
> MIRACLE LINUXのイメージをDownload  

  [MIRACLE LINUXダウンロードページ](https://www.miraclelinux.com/distribution/download)
![img.png](images/img.png)
   ※最新のミニマルインストールイメージをダウンロードする。
<br>
<br>
> ISOファイルを元に仮想マシンを作成

　![img_1.png](images/img_1.png)
　新規（N）を押下   
　![img_3.png](images/img_3.png)
　名前を設定、フォルダー、ISOイメージを選択する。   
　![img_5.png](images/img_5.png)
　ユーザ名、パスワードを設定して完了ボタンを押下。   
  お試し環境のためroot,rootを設定。   
  ※仮想マシンの起動に失敗する場合最下部の【補足】を参照
   <br>
   <br>
> ネットワークの設定   

以下の画面が表示されたら一度仮想マシンを停止してネットワークの設定を実施する。
![img_9.png](images/img_9.png)  
MiracleLinuxBaseの上で右クリック、Stop＞電源オフを押下     
![img_17.png](images/img_17.png)   
MiracleLinuxBaseの上で右クリック、設定を押下  
![img_18.png](images/img_18.png)
Expert ＞ ネットワーク ＞ アダプター1で【NAT】を選択   
（デフォルトで設定されているはず）
![img_19.png](images/img_19.png)
Expert ＞ ネットワーク ＞ アダプター2で【ネットワークアダプターを有効化】にチェックを入れ、【ブリッジアダプター】を選択、ブロードキャストモードで【すべて許可】を選択し、【OK】ボタンを押下   
![img_23.png](images/img_23.png)
   <br>
   <br>
> 仮想マシンを再起動する

　![img_21.png](images/img_21.png)
起動ボタンを押下
   <br>
   <br>
> 言語の選択  

![img_9.png](images/img_9.png)   
  日本語を選択して続行ボタンを押下
   <br>
   <br>
> rootパスワードの設定

![img_10.png](images/img_10.png)   
 rootパスワードを押下
![img_11.png](images/img_11.png)
 2箇所にパスワード【root】を入力して2度完了ボタンを押下
   <br>
   <br>
> インストール先の設定

![img_14.png](images/img_14.png)
インストール先を押下   
![img_15.png](images/img_15.png)
何も変更せずに完了ボタンを押下。
   <br>
   <br>
>  インストールの開始

![img_16.png](images/img_16.png)  
インストール開始ボタンを押下
<br />
<br />
> システムの再起動

「インストールが完了しました！」と表示されたら【システムの再起動】ボタンを押下
![img_20.png](images/img_20.png)
    <br />
    <br />
>  License情報の同意

![img_24.png](images/img_24.png)
次の順にキーを入力して進める。   
1を入力してEnterキーを押下   
2を入力してEnterキーを押下   
cを入力してEnterキーを押下   
qを入力してEnterキーを押下   
yesを入力してEnterキーを押下  
![img_25.png](images/img_25.png)
    <br />
    <br />
>  rootユーザでログイン

以下情報でログイン   
ユーザ【root】, パスワード【root】
![img_26.png](images/img_26.png)
---
## ユーザ作成など
> グループの作成
```
groupadd miracle
```
>  ユーザ作成
```
useradd -g miracle miracle
```
> パスワード変更
```
passwd　miracle
```
> sudoesを修正
```
sudo visudo
```
以下を最終行に追加
```
miracle ALL=(ALL)       NOPASSWD: ALL
```

>  net-toolsをインストール
```
sudo dnf install net-tools -y
```
---

## イメージのコピーを作成
>  仮想マシンの停止

MiracleLinuxBaseの上で右クリック ＞ Stop > 電源オフを押下
![img_27.png](images/img_27.png)
確認ダイアログで【電源オフ】を押下
<br />
<br />
> 仮想マシンのクローン作成(Controller)

MiracleLinuxBaseの上で右クリック ＞ クローンを押下
![img_28.png](images/img_28.png)   

以下を設定して【完了】ボタンを押下   
名前：ansibleController   
CloneType:リンクしたクローン   
MACアドレスのポリシー：NATネットワークアダプターのMACアドレスのみ含む
![img_32.png](images/img_32.png)
<br />
<br />
> 仮想マシンのクローン作成(target1, target2)

上記手順で名前を【target1】,【target2】として追加で２つクローンを作成する
![img_29.png](images/img_29.png)
---

## target1のホスト名を設定&ip確認
> target1を起動

   target1の上で右クリック ＞ 起動 ＞ 通常起動を押下
![img_30.png](images/img_30.png)   
<br />
> target1にVirtualBoxでログイン

ユーザ名 / パスワードに【miracle】を入力してログイン
![img_31.png](images/img_31.png)   
<br />
>  /etc/hostsにホスト名を追加する
```
sudo vi /etc/hosts
```
以下になるように修正して保存する
```
127.0.0.1   localhost target1
::1         localhost target1
```
![img_33.png](images/img_33.png)

> /etc/hostnameにホスト名を追加する
```
sudo vi /etc/hostname
```
以下になるように修正して保存する
```
target1
```
>  ipアドレスを確認
```
ifconfig
```
![img_34.png](images/img_34.png)  
<br />
>  仮想マシンの再起動
```
sudo shutdown now -r
``` 
<br />

> ホストマシン（Windows）からSSH接続

Powershellを起動して以下コマンドを入力  
※IPアドレスは環境によって変わります。   
yes/noの選択肢ではyesを入力してパスワード【miracle】を入力  
```
ssh miracle@上記で確認したIPアドレス
例）　ssh miracle@192.168.10.3
```
![img_36.png](images/img_36.png)

---

## target2のホスト名を設定&ip確認
>  target2を起動

   target2の上で右クリック ＞ 起動 ＞ 通常起動を押下
![img_37.png](images/img_37.png)   
   <br />
>  target2にVirtualBoxでログイン

   ユーザ名 / パスワードに【miracle】を入力してログイン
![img_38.png](images/img_38.png)
   <br />
>  /etc/hostsにホスト名を追加する
```
sudo vi /etc/hosts
```
以下になるように修正して保存する
```
127.0.0.1   localhost target2
::1         localhost target2
```

> /etc/hostnameにホスト名を追加する
```
sudo vi /etc/hostname
```
以下になるように修正して保存する
```
target2
```
> ipアドレスを確認
```
ifconfig
```
![img_39.png](images/img_39.png)
<br />

> 仮想マシンの再起動
```
sudo shutdown now -r
```

<br />

> ホストマシン（Windows）からSSH接続

   Powershellを起動して以下コマンドを入力  
   ※IPアドレスは環境によって変わります。   
   yes/noの選択肢ではyesを入力してパスワード【miracle】を入力
```
ssh miracle@上記で確認したIPアドレス
例）　ssh miracle@192.168.10.10
```
![img_40.png](images/img_40.png)

---

## ansibleControllerの設定
> ansibleControllerを起動

   ansibleControllerの上で右クリック ＞ 起動 ＞ 通常起動を押下
![img_41.png](images/img_41.png)
   <br />
>  ansibleControllerにVirtualBoxでログイン

   ユーザ名 / パスワードに【miracle】を入力してログイン
![img_42.png](images/img_42.png)  
   <br />
>  /etc/hostsにホスト名を追加する
```
sudo vi /etc/hosts
```
以下になるように修正して保存する
```
127.0.0.1   localhost ansible-controller
::1         localhost ansible-controller
```

> /etc/hostnameにホスト名を追加する
```
sudo vi /etc/hostname
```
以下になるように修正して保存する
```
ansible-controller
```
>  ipアドレスを確認
```
ifconfig
```
![img_43.png](images/img_43.png)
<br />

>  仮想マシンの再起動
```
sudo shutdown now -r
```

<br />

> ホストマシン（Windows）からSSH接続

   Powershellを起動して以下コマンドを入力  
   ※IPアドレスは環境によって変わります。   
   yes/noの選択肢ではyesを入力してパスワード【miracle】を入力
```
ssh miracle@上記で確認したIPアドレス
例）　ssh miracle@192.168.10.11
```
![img_44.png](images/img_44.png)

> ansibleをインストール
```
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm -y
sudo dnf update -y
sudo dnf install ansible -y
```
インストール後の確認
```
ansible --version
```
以下が表示されていればインストールOK
```
[miracle@ansible-controller ~]$ ansible --version
ansible [core 2.14.17]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/miracle/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3.9/site-packages/ansible
  ansible collection location = /home/miracle/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.9.21 (main, Dec  5 2024, 00:00:00) [GCC 11.5.0 20240719 (Red Hat 11.5.0-2)] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True
```
> ssh-keyを作成

ansible-controllerで以下コマンドを実行   
パスフレーズは設定しないのでEnterキーを２回押下
```
ssh-keygen -t rsa -f ~/.ssh/miracle
```
> 各サーバに公開キーを送信   

適宜、target1, target2のIPに変更すること
```
ssh-copy-id -i ~/.ssh/miracle miracle@192.168.10.3
ssh-copy-id -i ~/.ssh/miracle miracle@192.168.10.10
```
![img_45.png](images/img_45.png)
<br />
>  SSH接続確認   

target1にパスワード無しで接続
```
ssh -i ~/.ssh/miracle miracle@192.168.10.3
exit
```
ログインが成功すること<br />
<br />
target2にパスワード無しで接続
```
ssh -i ~/.ssh/miracle miracle@192.168.10.10
exit
```
ログインが成功すること<br />
<br />
---   

## Playbookを使用してtarget1, target2にnginx, firewalldをインストールする
> inventoryファイルを作成する
```
cd ~
mkdir ansible
cd ansible
vi inventory 
```
以下を入力して保存
```
target1 ansible_host=192.168.10.3 ansible_user=miracle ansible_ssh_private_key_file=/home/miracle/.ssh/miracle
target2 ansible_host=192.168.10.10 ansible_user=miracle ansible_ssh_private_key_file=/home/miracle/.ssh/miracle
```
> 接続確認   

target1にping実行して接続できるか確認する
```
 ansible -i inventory -m ping target1
```
以下の様にSUCCESSが返ってくれば接続できている
```
[miracle@ansible-controller ansible]$  ansible -i inventory -m ping target1
[WARNING]: Platform linux on host target1 is using the discovered Python interpreter at /usr/bin/python3.9, but future
installation of another Python interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-
core/2.14/reference_appendices/interpreter_discovery.html for more information.
target1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}
```
<br />

target2にping実行して接続できるか確認する
```
 ansible -i inventory -m ping target2
```
以下の様にSUCCESSが返ってくれば接続できている
```
[miracle@ansible-controller ansible]$ ansible -i inventory -m ping target2
[WARNING]: Platform linux on host target2 is using the discovered Python interpreter at /usr/bin/python3.9, but future
installation of another Python interpreter could change the meaning of that path. See https://docs.ansible.com/ansible-
core/2.14/reference_appendices/interpreter_discovery.html for more information.
target2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.9"
    },
    "changed": false,
    "ping": "pong"
}
```
> テンプレート作成
```
vi index.html.j2
```
以下を入力して保存
```
<!DOCTYPE html>
<html>
<body>
!!! This is {{ inventory_hostname }} Server !!!
</body>
</html>
```
> Playbook作成
```
vi start-playbook.yml
```
以下を入力して保存
```
---
  - hosts: all
    become: true
    tasks:
      - name: install package
        package:
          name:
            - nginx
            - firewalld
          state: installed
      - name: start nginx service
        service:
          name: nginx
          state: started
          enabled: yes
      - name: start firewalld service
        service:
          name: firewalld
          state: started
          enabled: yes
      - name: insert firewalld rule
        firewalld:
          port: 80/tcp
          zone: public
          state: enabled
          permanent: yes
          immediate: yes
      - name: Copy index.html to remote servers
        template:
          src: index.html.j2
          dest: /usr/share/nginx/html/index.html
```
>  playbookを実行   

※/home/miracle/ansibleで実行すること
```
ansible-playbook -i inventory start-playbook.yml
```
エラーなく終わること
```
PLAY RECAP ****************************************************************************
target1                    : ok=6    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
target2                    : ok=6    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
> ブラウザで結果を確認   

target1のIPをブラウザで呼び出す。<br />
![img_46.png](images/img_46.png)<br />

target2のIPをブラウザで呼び出す。<br />
![img_47.png](images/img_47.png)<br />

### 呼び出しができればAnsibleからnginxとfirewalldの設定が問題なくできたので確認完了

---
## installしたnginx, firewalldを削除
> 削除用のPlaybookを作成
```
vi clear-playbook.yml
```
以下を入力
```
---
  - hosts: all
    become: true
    tasks:
      - name: stop nginx service
        service:
          name: nginx
          state: stopped
      - name: stop firewalld service
        service:
          name: firewalld
          state: stopped
      - name: delete package
        package:
          name:
            - nginx
            - firewalld
          state: absent
```
> Playbookを実行

```
 ansible-playbook -i inventory clear-playbook.yml
```
エラーなく終わること
```
PLAY RECAP ****************************************************************************
target1                    : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
target2                    : ok=4    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
>  ブラウザで結果を確認<br />

target1のIPをブラウザで呼び出す。  
アクセスできなければOK  <br /><br/>
target2のIPをブラウザで呼び出す。  
アクセスできなければOK
---
##  補足：仮想マシン起動時にエラーになった場合
![img_8.png](images/img_8.png)   
以下コマンドを管理者権限のPowerShallで実行する
```
bcdedit /enum | findstr "hypervisorlaunchtype"
```
結果が次の場合、Hyper-V がオンになっていて、VirtualBox の仮想マシン起動で問題が起こる。
```
hypervisorlaunchtype    Auto
```
何も表示されなかったり、次のように Off の表示の場合は、Hyper-V がオフになっている。   
この場合で、VirtualBox で仮想マシンが起動できない場合は、この対処では解決できない。
```
hypervisorlaunchtype    Off
```
### 解決方法
次のコマンドを実行しHyper-V をオフにする。
```
bcdedit /set hypervisorlaunchtype off
```  
実行後Windowsを再起動し再度VirtualBoxで仮想マシンが起動できるか確認する。

---


