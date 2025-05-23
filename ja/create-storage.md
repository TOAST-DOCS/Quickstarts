# ストレージの作成と接続
**Quickstarts > 7.ストレージの作成および接続**

今回の学習モジュールでは、NHN Cloudコンソールを通じてストレージサービスを有効化し、使用する方法をご案内します。NHN Cloudの**ストレージサービスは、**データの保存と管理に必要な安定的で拡張可能なソリューションを提供します。

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%97%B0%EA%B2%B0.png)
## 学習目標

今回の学習モジュールで学ぶ内容は以下の通りです。

* **Storageサービスの活用**
    * Object StorageとBlock Storageの違いとユースケースの比較
    * Block Storage ボリュームの作成とインスタンスとの接続
    * Object Storage コンテナの作成とオブジェクトのアップロードとダウンロード
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/%EB%AA%A8%EB%93%88%207.%20%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%97%B0%EA%B2%B0.png)

<p style="text-align: center; color: black;">最終構成図</p>

## 始める前に

NHN Cloudを開始するためには、次の事項を準備する必要があります。

* **標準のインターネットブラウザ**
    * Google Chrome、Microsoft Edge、Firefox、Safariなどの最新バージョンのブラウザがインストールされている必要があります。
    * ブラウザの設定でJavaScriptとCookieが有効になっている必要があります。
* **インターネット接続環境**
    * 安定したインターネット接続が必要で、推奨帯域幅は最低5Mbps以上です。
    * HTTPSによる安全な通信が可能であること。
* **会員アカウント**
    * 決済手段を登録したNHN Cloudアカウントが必要です。
    * NHN Cloudのホームページにログインする必要があります。

**本ガイドは、[6.データベースの作成と接続](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/create-database/)以降の段階から始まります。**

## ブロックストレージの作成とデータ照会

> ブロックストレージを生成してLinuxインスタンスに接続した後、ブロックストレージのデータを照会します。

### ステップ1.ブロックストレージを作成した後、Linuxインスタンスに接続する

> モジュール3で生成したLinuxインスタンス`linux-server-basicに`ブロックストレージリソースを接続します。

1. コンソールウィンドウの左側のメニューから**Storage - Block Storageを**クリックします。
2. **Block Storage > 管理画面で、**ブロックストレージ一覧のうち、接続情報が**linux-server-basicの/dev/vdaで**あるブロックストレージの可用性領域を確認します。
3. **+ ブロックストレージの作成を**クリックします。
4. **ブロックストレージ作成**ウィンドウで以下の情報を設定し、[**OK**]をクリックします。
    * ブロックストレージ名:`MyBS`
    * ブロックストレージタイプ:`HDD`
    * ブロックストレージサイズ(GB)：`10 GB`
    * 可用性領域：ブロックストレージリストで接続情報が`linux-server-basicの/dev/vdaである`ブロックストレージと**同じ可用性領域。**
5. 成功]ウィンドウで[**OK**]をクリックします。
6. `MyBSを`選択した後、上の**接続管理を**クリックします。
7. **ブロックストレージ接続管理**ウィンドウで以下の情報を設定した後、**接続を**クリックします。
    * インスタンスへの接続:`linux-server-basic`
8. 成功]ウィンドウで[**OK**]をクリックします。

### ステップ2.ブロックストレージパーティションの追加設定、フォーマット、マウントを行う

* Linuxインスタンス`linux-server-basicに`リモート接続した状態で下記のコマンドを実行して、ステップ1で生成したブロックストレージ`MyBSに`パーティションを生成した後、フォーマットとマウントを実行します。
```
#bash
echo -e "n\np\n1\n\n\nw" | sudo fdisk /dev/vdb
```

!!! tip "知っておくべきこと"
\* ドメイン名ガイド
\* fdisk コマンドの説明
\* fdisk **`n`**: 新しいパーティションの作成 (オプション:`n)`
\* p **`p`**: パーティションタイプの選択 (デフォルト: Primary)
\*(オプション) **`1`**: パーティション番号 (デフォルト: 1番)
\***最初のセクタ**：Enterを押してデフォルト値を選択します。
\***最後のセクタ**：Enterを押してデフォルトの最大サイズを選択します。
\*最大サイズ **`w`**: 変更内容を保存して終了

* 以下のコマンドを実行してパーティションをフォーマットします。
```bash
sudo mkfs -t xfs /dev/vdb1
```

* 追加したパーティションを/mnt/vdbパスにマウントできるように下記のコマンドを実行して/mnt/vdbディレクトリを生成します。
```
sudo mkdir /mnt/vdb
```

* 起動時に追加したパーティションを自動マウントするように下記のコマンドを実行して/etc/fstabファイルに設定を追加します。
```
sudo sh -c 'echo "UUID=$(sudo blkid -s UUID -o value /dev/vdb1) /mnt/vdb xfs defaults,nodev,noatime,nofail 1 2" >> /etc/fstab'
```

* 下記のコマンドを実行して/etc/fstabファイルで定義された全てのファイルシステムをマウントし、全てのユーザーが/mnt/vdbにファイルを作成、修正、削除できるように権限を変更します。
```bash
sudo mount -a
sudo chmod 777 /mnt/vdb
```

* 以下のコマンドを入力して、追加ブロックストレージが正しく設定されていることを確認します。
```bash
df /dev/vdb1
```

**該当Filesystemと容量、Mountパスが照会される**ことを確認します。
<br></br>

![pic1](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%8A%A4%ED%86%A0%EB%A0%88%EC%A7%80%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%84%A4%EC%A0%95_%EC%9E%91%EC%97%852.png)

<p style="text-align: center; color: black;">出力画面</p>

### ステップ3.追加のブロックストレージに資料を作成する

* `linux-server-basicに`リモート接続した状態で下記のコマンドを実行してブロックストレージを生成します。
```bash
export MYSQL_PWD=nhnpassword
```

* 生成したブロックストレージに`mysql-db-basic`データ照会資料を生成します。
```bash
mysql --host=(mysql-db-basic インスタンスの仮想 IP アドレス) --user=nhncloud -e "SELECT * FROM employees.employees LIMIT 30;" -B --batch > /mnt/vdb/employees.csv
```

* 下記のコマンドで生成したデータ(<span style="color:rgb(0, 82, 204);"><strong>csvファイル)</strong></span>を確認します。
```bash
cat /mnt/vdb/employees.csv
```

![pic2](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%84%A4%EC%A0%95_%EC%9E%91%EC%97%853.png)

<p style="text-align: center; color: black;">出力画面</p>

## オブジェクトストレージの作成とデータ照会

> オブジェクトストレージサービスを有効にしてコンテナを生成した後、オブジェクトをアップロードします。そのオブジェクトをLinuxインスタンスに保存した後、接続してデータが出力されるか確認します。

### ステップ1.オブジェクト・ストレージ・サービスを有効化し、コンテナを作成する

1. `MyPRJ`プロジェクトの右側にある**「サービス選択」**タブをクリックします。
2. **サービス選択を**クリックして出てくる画面で、**すべてのサービス - Storage - Object Storageを**クリックします。
3. **サービス有効化**ウィンドウで[**OK**]をクリックします。
4. サービスが正しく有効化されると、Object Storage画面に移動します。もし、画面が移動しない場合は、コンソールウィンドウの左側のメニューから**Storage - Object Storageを**クリックします。
5. **+ コンテナ作成を**クリックします。
6. **コンテナ作成**ウィンドウで以下の情報を入力し、[**OK**]をクリックします。
    * 名前:`myobs`
    * アクセスポリシー:`PUBLIC`
    * ストレージクラス:`Standard`
    * オブジェクトロック設定:`無効`
    * 暗号化設定:`無効`
7. 成功]ウィンドウで[**OK**]をクリックします。

### ステップ2.オブジェクトストレージを利用してLinuxインスタンスのWebソースを変更する

* ユーザーの作業環境で新しい**ターミナル**または**PowerShellを**実行します。
* 下記のコマンドでweb-sampleディレクトリを生成してindex.htmlファイルを保存します。
```
#PowerShell
mkdir /web-sample
```

```PowerShell
    echo '<!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>::: Welcome to NHN Cloud :::</title>
        <script>
            let countdownTimer;
    
            function fetchServerIP() {
                fetch("/server-info")
                    .then(response => response.json())
                    .then(data => {
                        const serverIP = data.serverIP;
                        document.getElementById("server-ip").innerText = "Server IP Address: " + serverIP;
                    })
                    .catch(error => {
                        document.getElementById("server-ip").innerText = "Unable to fetch server IP address";
                    });
            }
    
            function startStressTest() {
                const button = document.getElementById("start-btn");
                button.innerText = "Starting...";
                let timeLeft = 120;
                const countdownDisplay = document.getElementById("countdown");
                countdownDisplay.innerText = "Time left: " + timeLeft + " seconds";
    
                countdownTimer = setInterval(() => {
                    timeLeft--;
                    countdownDisplay.innerText = "Time left: " + timeLeft + " seconds";
    
                    if (timeLeft <= 0) {
                        clearInterval(countdownTimer);
                        countdownDisplay.innerText = "Stress test complete!";
                    }
                }, 1000);
    
                fetch("/start-stress")
                    .then(response => response.text())
                    .then(data => {
                        console.log(data);
                    })
                    .catch(error => {
                        console.error("Error starting stress test:", error);
                    });
            }
    
            window.onload = fetchServerIP;
        </script>
    </head>
    <body>
        <h1>Welcome to NHN Cloud Web Server</h1>
        <p id="server-ip">Fetching server IP address...</p>
        <button id="start-btn" onclick="startStressTest()">Start Stress Test</button>
        <p id="countdown">Time left: 120 seconds</p>
    </body>
    </html>' > /web-sample/index.html
```

3. コンソールウィンドウの左側のメニューから**Storage - Object Storageを**クリックします。
4. Object Storage画面で`myobsを`クリックしてコンテナ詳細ページ画面に移動します。
5. `myobs`画面で**オブジェクトのアップロードを**クリックします。
6. **オブジェクトアップロードウィンドウで** **ファイル選択を**クリックした後、**/web-sampleに**ある`index.html`ファイルを選択し、[**OK**]をクリックします。
7. **アップロードステータス情報**ウィンドウで`index.html`ファイルのアップ**ロード**状態が**成功である**ことを確認した後、[**OK**]をクリックします。
8. アップロードした`index.html`オブジェクトの右側にある**Public URL**項目にある**Copy** **URLを**クリックして、**index.htmlファイルのPublic URLアドレスを**コピーします。
9. `linux-server-basicに`リモートアクセスをします。
> [参考】`linux-server-basicへの`リモートアクセス方法
>
> * このリモート接続方法は[、4.ネットワーク設定とインスタンス作成](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/network-setup/)-**ステップ1.SSHリモート接続を**参照してください。
10. `linux-server-basic`リモート接続後、下記のコマンドを実行してindex.htmlをダウンロードして保存します。
```bash
sudo curl -o /var/www/html/index.html (myobs にアップロードした index.html ファイルの Public URL)
```

!!! tip "知っておきたいこと"
\* Nginxの基本ウェブドキュメントパス
\* UbuntuでインストールしたNginxの基本ウェブ文書のパスは`/var/www/htmlです。` `http://복사한 linux-server-basic フローティングIPアドレスに`接続する時、出力されるウェブページ文書は`/var/www/html/index.html`です。

* 以下のコマンドを実行して、追加サービス構成を設定します。

```bash
curl -s https://kr2-api-object-storage.nhncloudservice.com/v1/AUTH_cd41d4b57c1346b99641cc4092f2842d/onboarding/service-setting.sh | sed 's/\r$//' > /home/ubuntu/service-setting.sh
chmod +x /home/ubuntu/service-setting.sh
/home/ubuntu/service-setting.sh
```

* ウェブブラウザで新しいウィンドウを開いて`http://복사한 linux-server-basic フローティングIPアドレスを`入力して、変更されたウェブページを確認します。
![pic3](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80%20%EC%83%9D%EC%84%B1_%EB%8B%A8%EA%B3%843.png)

!!! tip "知っておくべきこと"
\* 変更されたウェブページが表示されない場合
\* ウェブページが変更されたにもかかわらず、以前のページが表示される場合は、ウェブブラウザのキャッシュが原因で発生した問題の可能性があります。この場合、ブラウザが提供するリフレッシュ(F5)または強力リフレッシュを使用して変更されたページを確認することができます。強制更新は、次の方法で実行することができます。
\***Windows**:`Ctrl + F5`または`Shift + F5`
\***Mac**:`Cmd + Shift + R`


## 参考資料

* [Storage](https://en.wikipedia.org/wiki/Cloud_storage)
* [Block Storage](https://docs.nhncloud.com/ko/Storage/Block%20Storage/ko/overview/)
* [Object Storage](https://docs.nhncloud.com/ko/Storage/Object%20Storage/ko/Overview/)
* [HDD](https://en.wikipedia.org/wiki/Hard_disk_drive)
* [SSD](https://en.wikipedia.org/wiki/Solid-state_drive)
* [Disk encryption](https://en.wikipedia.org/wiki/Disk_encryption)
* [fdisk](https://en.wikipedia.org/wiki/Fdisk)
* [DF](https://en.wikipedia.org/wiki/Df_(Unix))
* [Mount](https://en.wikipedia.org/wiki/Mount_(computing))
* [chmod](https://en.wikipedia.org/wiki/Chmod)

## 前の段階

* [06-データベースの作成と接続](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/create-database/)

## 次のステップ

* [08-モニタリング設定](https://docs.alpha-nhncloud.com/ko/quickstarts/ko/create-database/)
