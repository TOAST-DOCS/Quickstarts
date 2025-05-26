# ネットワーク設定とインスタンス作成
**Quickstarts > 4.ネットワーク設定とインスタンス作成**

今回の学習モジュールでは、NHN CloudでLinuxベースのWebサーバーを作成し、リモートアクセスして駆動する方法を説明します。NHN Cloudは、ユーザーフレンドリーなインターフェースと多様なクラウドリソースを通じて、誰でも簡単に安定的で効率的なIT環境を構築することができます。

![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%20%EC%84%A4%EC%A0%95%EA%B3%BC%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1.png)
## 学習目標

今回の学習モジュールで学ぶ内容は以下の通りです。 

* **インスタンス作成**
    * UbuntuサーバーをベースにしたLinuxインスタンスの作成
    * 基本的なサーバー環境を構築
    * インスタンスタイプ、キーペア設定、セキュリティグループおよびネットワーク設定を含む詳細設定
* **ネットワーク設定**
    * VPC、サブネット、ルーティングテーブルの設定
* **SSHリモートアクセス**
    * 生成されたインスタンスにSSH経由でリモートアクセス
* **Webサーバーのインストールと実行**
    * Webサーバーのインストールと駆動
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/%EB%AA%A8%EB%93%88%204.%20%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC%20%EC%84%A4%EC%A0%95%EA%B3%BC%20%EC%9D%B8%EC%8A%A4%ED%84%B4%EC%8A%A4%20%EC%83%9D%EC%84%B1.png)

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

**本ガイドは、[3.IAMアカウントとガバナンスの設定](https://docs.nhncloud.com/ja/quickstarts/ja/iam-accounts/)以降の段階から始めます。**

## インスタンス作成の準備

### ステップ1.基本インフラサービスを有効にする

1. NHN Cloudコンソールに接続した後、上部のメニューで実習に使用する組織`(MyORG)`、プロジェクト(`MyPRJ)`、そして`韓国(平村)リージョンを`選択したことを確認します。
2. `MyPRJの`右側にある**サービス選択を**クリックします。
3. **サービスの選択を**クリックした後、**Compute - Instanceを**クリックします。
4. **サービス有効化**ウィンドウで[**OK**]をクリックします。

!!! tip "知っておくべきこと"
    * サービスの有効化
        * サービスを有効化すると、NHN Cloudコンソールプロジェクト画面の左側にサービスメニューが表示されます。そのメニューで必要なリソースを管理することができます。
        * 設定したリージョンによって、一部のサービスは提供されない場合があります。
        * Instance サービスを有効化すると、関連する基本インフラサービスが一緒に有効化されます。

### ステップ2.基本的なネットワーク設定

> インスタンスのリモート接続に必要な基本的なネットワークリソースのVPC、サブネット、ルーティングテーブルの名前を設定します。

1. 左側のメニューから**Network - VPCを**クリックします。
2. `Default Network`(CIDR: 192.168.0.0/16) を選択した後、**VPC変更を**クリックします。
3. **VPC変更**ウィンドウで以下の内容を入力し、[**OK**]をクリックします。
    * 名前:`MyVPC`
4. 成功]ウィンドウで[**OK**]をクリックします。
5. 左側のメニューから**Network - Subnetを**クリックします。
6. `Default Network`(CIDR: 192.168.0.0/24) を選択した後、**サブネットの変更を**クリックします。
7. **サブネット変更**ウィンドウで以下の内容を入力し、[**OK**]をクリックします。
    * 名前:`MySubnet`
8. 成功]ウィンドウで[**OK**]をクリックします。
9. 左側のメニューから**Network - Routingを**クリックします。
10. vpc-(任意の名前の値)を選択した後、**ルーティングテーブルの変更を**クリックします。
11. **ルーティングテーブルの変更**ウィンドウで以下の内容を入力し、[**OK**]をクリックします。
    * 名前:`MyRT`
12. 成功]ウィンドウで[**OK**]をクリックします。

### ステップ3.Linuxインスタンスを作成する

1. 左メニューの**Compute - Instanceを**クリックします。
2. **インスタンス作成を**クリックします。
3. **インテンション作成**ウィンドウで以下の情報を設定した後、**インスタンス作成を**クリックします。
    * インスタンステンプレート
        * 使用:`未使用`
    * イメージ
        * パブリックイメージ > イメージ名：`Ubuntu Server 22.04 LTSを`クリック(パブリックイメージ一覧の下部に該当するイメージがあります。)
    * インスタンス情報
        * 可用性領域：`任意の可用性領域`
        * インスタンス名:`linux-server-basic`
        * インスタンスタイプ:**インスタンスタイプ選択**クリック > インスタンスタイプ名:`t2.c1m1`(分類: Standard, タイプ: t2, 名前: t2.c1m1, vCPU: 1, メモリ: 1GB) クリック後**選択**クリック
        * インスタンス数：`1`
        * キーペア >**+作成を**クリック > キーペア選択下部に`MyKeyを`入力後、キー**ペアを作成**> キー**ペアをダウンロードを**クリック後、`MyKey.pem`ファイルをダウンロード
    * ルートブロックストレージ
    * ブロックストレージタイプ:`HDD`
    * ブロックストレージサイズ(GB)：`20`GB
    * ネットワーク設定：`ネットワークインターフェースの作成を`選択
    * ネットワーク
        * 利用可能なサブネット項目で`MySubnet (192.168.0.0/24)`リソースをクリックして、選択したサブネットとして使用します。
    * フローティングIP：**設定変更を**クリック
        * 使用:`使用`
        * 削除保護:`無効`
        * 接続するサブネット:`MySubnet (192.168.0.0/24)`
    * セキュリティグループ
        * **セキュリティグループ作成を**クリック
        * **セキュリティグループ作成**ウィンドウで以下の情報を設定し、[**OK**]をクリックします。
            * 名前:`MySG-SSH`
            * セキュリティルールの追加に\*\*+**をクリックして、次の設定のセキュリティルールを追加します。
                * 方向:`受信`
                * IPプロトコル:`SSH`
                    * 該当するIPプロトコルを選択すると、自動的にポート情報が入力されます。
                    > [参考] カスタムTCP
                    >
                    > * IPプロトコルを「ユーザー定義TCP」を選択すると、ポート値をユーザーが直接入力することができます。**1と**表記された入力欄に`22を`入力します。
                * Ether: `IPv4`
                * リモート:`CIDR - 0.0.0.0/0/0`
        * 成功ウィンドウの[**OK**]をクリック
        * **セキュリティグループ選択**項目で上記で作成した`MySG-SSHを`選択します`。`
    * 追加ブロックストレージ: 未使用(デフォルト)
    * ユーザースクリプト: 空欄(基本)
    * 削除保護: 無効(デフォルト)
4. インスタンス作成情報ウィンドウで**インスタンス作成を**クリックします。
5. インスタンス作成作業が行われます。そのインスタンスは数分程度で作成が完了します。

!!! tip "知っておきたい"
    * 簡単な公開画像検索のヒント
        * **公開画像タブの**下部にある「全画像」をクリックした後、ドロップダウンメニューから`OS → Ubuntuを`選択して対象画像を確認し、選択します。
        * **公開イメージタブの**下部に "全体イメージ" 右側にある "イメージ名または説明を入力してください" 入力欄に`Ubuntu Server 22.04`を入力して対象イメージを確認後選択
    * キーペア管理
        * キーペアのダウンロードは、キーペア作成時に一度だけダウンロードすることができます。ファイルを安全な場所にダウンロードして管理してください。
        * 既に作成した`MyKey`キーペアが"キーペア選択"ドロップダウンメニューにある場合は、そのキーを選択してください。ただし、MyKey.pemファイルをユーザーが覚えているパス(ディレクトリまたはフォルダ)にダウンロードしておいた状態でなければなりません。
        * キーペアを作成すると、自動的にそのキーペア値が選択されます。
        * 詳細は、[キーペアユーザーガイドを](https://docs.nhncloud.com/ja/Compute/Instance/ja/overview/#key-pair)ご参照ください。

## インスタンス接続とNginxウェブサーバー駆動

### ステップ1.SSHリモート接続する

1. 左側のメニューから**Network - Floating IP を**クリックします。
2. フローティングIPリソースのリストのうち、接続されたデバイスが`linux-server-basicである`IPアドレスを**コピーして** **記録します。**
3. 下記のユーザー環境に合わせてSSHリモート接続を行います。

### Windowsを使用する場合

* Windowsの**スタートを**クリックし、`Windows PowerShellを`検索して実行します。

!!! tip "知っておくべきこと"
    * Windows PowerShell実行のヒント
        * `ウィンドウキー(⊞) + R`を押して実行ウィンドウを実行します。
        * 開く(O)入力欄に`powershell.exe`を入力した後、Enterキーを押すと、Windows PowerShellが実行されます。

* ダウンロードしたキーペアである`MyKey.pem`フォルダに移動します。

```
#Powershell
cd \(MyKey.pem ファイルがあるフォルダ名)
```
      
* MyKey.pemの権限を初期化します。
```
icacls MyKey.pem /reset
```        

* PowerShellを実行したユーザーのアカウントに`MyKey.pem`権限を付与します。
```
icacls MyKey.pem /grant:r "$($env:username):(r)"
```

* MyKey.pemファイルが親フォルダから権限を継承しないように設定します。
```
icacls MyKey.pem /inheritance:r
```

!!! tip "知っておくべきこと"
    * SSH キーやセキュリティ証明書ファイルは厳密に権限を制限する必要があります。Windowsでは、icaclsコマンドを使用して継承をブロックし、ユーザー別読み取り権限を付与してセキュリティレベルを維持します。

* sshコマンドでインスタンスに接続します。
```
#PowerShell
ssh -i MyKey.pem ubuntu@コピーしたlinux-server-basicフローティングIPアドレス
```

* コピーしたIPアドレス(上記の手順で確認したフローティングIPアドレスです。)
* "Are you sure you want to continue connecting (yes/no/[fingerprint])?" というフレーズが出たら、`yesを`入力後、Enterキーを押します。

* lsb_releaseコマンドで現在接続しているLinuxのバージョンを確認します。
```
#bash
lsb_release -a
```
            
### macOSをご利用の場合

* Dockで**ターミナル(Terminal)**アプリを実行するか、Spotlightで**ターミナルを**検索して実行します。

!!! tip "知っておくべきこと"
    * ターミナル実行のヒント
        *`Command(⌘) + Space`を押してSpotlightを実行します。
        * 入力欄に`ターミナルを`入力した後、<span style="color:rgb(51, 51, 51);">Returnキーを</span>押すとターミナルが実行されます。

* ダウンロードしたキーペアである`MyKey.pem`ディレクトリに移動します。
```
#bash
cd /(MyKey.pem ファイルがあるディレクトリ名)
```

* 下記のコマンドを入力してMyKey.pemの権限を設定します。
```
chmod 600 MyKey.pem
```

* sshコマンドでインスタンスに接続します。
```
#PowerShell
ssh -i MyKey.pem ubuntu@コピーしたlinux-server-basicフローティングIPアドレス
```

* "Are you sure you want to continue connecting (yes/no/[fingerprint])?" と表示されたら`yes`を入力した後、Enterキーを押します。

* lsb_releaseコマンドで現在接続しているLinuxのバージョンを確認します。
```
#bash
lsb_release -a
```

### ステップ2.ウェブサーバーをインストールして起動する

* インスタンスにリモート接続した状態で下記のコマンドを入力してNginxウェブサーバーをインストールします。
```
#bash
sudo apt update -y
sudo apt install nginx -y
```

* インストールが完了したら、下記のコマンドでウェブサーバーが動作していることを確認します。
```
#bash
curl localhost
```
    
![4 ネットワーク設定とインスタンス作成_作業5 スクリーンショット_rv1](https://github.com/user-attachments/assets/3bc07c8b-d6e2-431d-aad4-da3bfa6562e4)

## 参考資料

* [リージョンガイド](https://docs.nhncloud.com/ja/nhncloud/ja/region-guide/)
* [Compute Instance](https://docs.nhncloud.com/ja/Compute/Instance/ja/overview/)
* [System Image](https://en.wikipedia.org/wiki/System_image)
* [VPC](https://docs.nhncloud.com/ja/Network/VPC/ja/overview/)
* [Subnet](https://docs.nhncloud.com/ja/Network/VPC/ja/console-guide/#_4)
* [Floating IP](https://www.nhncloud.com/kr/service/network/floating-ip)
* [CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)
* [Public-key暗号化](https://en.wikipedia.org/wiki/Public-key_cryptography)
* [SSH](https://en.wikipedia.org/wiki/Secure_Shell)
* [VM](https://en.wikipedia.org/wiki/Virtual_machine)
* [Internet Gateway](https://docs.nhncloud.com/ja/Network/Internet%20Gateway/ja/overview/)
* [Nginx](https://en.wikipedia.org/wiki/Nginx)
* [ウェブサーバー](https://en.wikipedia.org/wiki/Web_server)
* [Linux](https://en.wikipedia.org/wiki/Linux)
* [Network Interface](https://docs.nhncloud.com/ja/Network/Network%20Interface/ja/overview/)

## 前の段階

* [3. IAMアカウントとガバナンスの設定](dooray://1387695619080878080/pages/3977493217025617647 "publish")


## 次のステップ

* [5. -セキュリティ設定](dooray://1387695619080878080/pages/3959371258218176884 "publish")
