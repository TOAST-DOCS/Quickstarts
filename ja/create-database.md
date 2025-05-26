# データベースの作成と接続
**Quickstarts > 6.データベースの作成と接続**

今回の学習モジュールでは、NHN Cloud環境でデータベースを作成し、アプリケーションと接続する基本的な構成手順をご案内します。NHN Cloudでは、安定的で拡張可能な**Databaseサービスを**提供し、ユーザーが簡単かつ効率的にデータベースを構築して運営できるように支援します。

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%97%B0%EA%B2%B0.png)
## 学習目標

今回の学習モジュールで学ぶ内容は以下の通りです。

* **Databaseサービスの活用**
    * データベースインスタンスの作成と初期設定
    * データベースへのアクセスと簡単なデータ操作
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/%EB%AA%A8%EB%93%88%206.%20%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%97%B0%EA%B2%B0.png)

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
    * NHN Cloudポータルにログインする必要があります。

    **本ガイドは[5.セキュリティ設定](https://docs.nhncloud.com/ja/quickstarts/ja/configure-security/)以降の手順から始まります。**

## データベース作成とデータ照会

### ステップ1.MySQLデータベースインスタンスの作成

1. NHN Cloudコンソール上部のメニューから実習に使用する組織`(MyORG)`、プロジェクト(`MyPRJ)`、そして`韓国(平村)リージョンを`選択します。
2. コンソールウィンドウの左側のメニューから**Database - MySQL Instanceを**クリックします。
3. **インスタンス作成を**クリックします。
4. **インスタンス作成**ウィンドウで以下の情報を設定後、**インスタンス作成を**クリックします。
    * インスタンステンプレート
        * 使用:`未使用`
    * イメージ
        * パブリックイメージ > イメージ名:`Ubuntu Server 20.04 LTS`クリック
    * インスタンス情報
        * 可用性領域：`任意の可用性領域`
        * インスタンス名:`mysql-db-basic`
        * インスタンスタイプ:**インスタンスタイプ選択**> インスタンスタイプ名から`t2.c1m1を`クリックし、**選択を**クリックします。
        * インスタンス数：`1`
        * キーフェア >`MyKey`
            * キーペアを追加で作成して使用する場合は、[キーペアユーザーガイドを](https://docs.nhncloud.com/ja/Compute/Instance/ja/console-guide/#_21)参照してください。
    * ルートブロックストレージ
        * ブロックストレージタイプ:`HDD`
        * ブロックストレージサイズ(GB)：`20`GB
    * ネットワーク設定：`ネットワークインターフェースの作成を`選択
    * ネットワーク
        * 利用可能なサブネット項目で`MySubnet(192.168.0.0.0/24)`リソースをクリックし、選択したサブネットとして使用します。
    * フローティングIP：**設定変更を**クリック
        * 使用:`未使用 (Default)`
    * セキュリティグループ
        * **セキュリティグループ作成を**クリック
        * **セキュリティグループ作成**ウィンドウで以下の情報を設定し、[**OK**]をクリックします。
            * 名前:`MySG-DB`
            * セキュリティルールの追加で\*\*+**をクリックした後、次の設定のセキュリティルールを追加します。
                * 方向:`受信`
                * IPプロトコル:`My SQL`
                    * 該当するIPプロトコルを選択すると、自動的にポート情報が入力されます。
                    > [参考] カスタムTCP
                    >
                    > * IPプロトコルを「ユーザー定義TCP」を選択すると、ポート値をユーザーが直接入力することができます。**1と**表記された入力欄に`3306を`入力してください。
            * Ether: `IPv4`
            * リモート:`CIDR - 192.168.0.0.0/24`
        * 成功]ウィンドウで[**OK**]をクリックします。
        * **セキュリティグループ選択**項目で上記で作成した`MySG-DBを`選択します。
    * 追加ブロックストレージ: 無効 (デフォルト)
    * ユーザースクリプト
        * [ダウンロード](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/create-database-script.txt)
    * 削除保護: 無効 (デフォルト)
4. インスタンス作成情報ウィンドウで**インスタンス作成を**クリックします。
5. インスタンス作成作業が行われます。このインスタンスは約1分程度で作成が完了します。
6. インスタンス作成後、`mysql-db-basicである`IPアドレスを**コピーして** **記録します。**

### ステップ2.Linuxインスタンスでデータベース接続をテストします。

> モジュール4で生成した`linux-server-basic`インスタンス`linux`-server-basicを使ってデータベースに接続します。 linux-server-basicインスタンスの生成[及び](https://docs.nhncloud.com/ja/quickstarts/ja/network-setup/)接続方法は[04-ネットワーク設定とインスタンス生成を](https://docs.nhncloud.com/ja/quickstarts/ja/network-setup/)参照してください。

1. 新しい**ターミナル**または**PowerShellを**実行します。
2. 下記のコマンドで`linux-server-basicに`リモート接続します。

```
#PowerShell
cd /(MyKey.pem ファイルがあるフォルダ名)
```
```
ssh -i MyKey.pem ubuntu@(linux-server-basicインスタンスのFloating IPアドレス)
```

* リモート接続環境で下記のコマンドを実行して、**MySQL-Client**ツールをインストールします。
```
#bash
sudo apt update 
sudo apt install mysql-client -y
```

* 下記のコマンドを実行して、データベースの結果値を確認します。
```
#bash
export MYSQL_PWD=nhnpassword
```
```
mysql --host=(mysql-db-basic インスタンスの仮想 IP アドレス) --user=nhncloud -e "SELECT * FROM employees.employees LIMIT 30;"
```

**データベースの結果値が照会される**ことを確認します。
<br></br>
![pic1](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1%20%EB%B0%8F%20%EC%97%B0%EA%B2%B0_%EC%9E%91%EC%97%852.png)

## 参考資料

* [MySQL](https://en.wikipedia.org/wiki/MySQL)
* [Database](https://en.wikipedia.org/wiki/Database)
* [SQL](https://en.wikipedia.org/wiki/SQL)
* [RDS for MySQL](https://docs.nhncloud.com/ja/Database/RDS%20for%20MySQL/ja/overview/)

## 前の段階

* [5.セキュリティ設定](https://docs.nhncloud.com/ja/quickstarts/ja/configure-security/)

## 次のステップ

* [7.ストレージの作成と設定](https://docs.nhncloud.com/ja/quickstarts/ja/create-storage/)
