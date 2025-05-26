# セキュリティ設定
**Quickstarts > 5.セキュリティ設定**

今回の学習モジュールでは、NHN Cloudでセキュリティを設定・管理する基本概念と主要機能を段階的に案内し、安全で信頼できるクラウド環境を構築する方法を学習します。NHN Cloudはユーザーのデータを安全に保護し、クラウドリソースを効率的に管理できる様々なセキュリティ機能を提供しています。
![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%B3%B4%EC%95%88%20%EC%84%A4%EC%A0%95.png)

## 学習目標

今回の学習モジュールで学ぶ内容は以下の通りです。

* **ネットワークセキュリティ設定**
    * VPCおよびサブネット構成によるネットワーク分離と保護
    * セキュリティグループとACL(Access Control List)を通じて許可されたトラフィックのみを許可します。
* **データセキュリティ**
    * NHN Cloudが提供するデータ暗号化方式とストレージのセキュリティ設定
    * データ損失に備えるためのバックアップとリカバリ戦略
<br></br>

![mod_diagram](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/%EB%AA%A8%EB%93%88%205.%20%EB%B3%B4%EC%95%88%20%EC%84%A4%EC%A0%95.png)

<p style="text-align: center; color: black;">最終構成図</p>

## 始める前に

NHN Cloudを開始するためには、次の事項を準備する必要があります。

* **標準のインターネットブラウザ**
    * Google Chrome、Microsoft Edge、Firefox、Safariなどの最新バージョンのブラウザがインストールされている必要があります。
    * ブラウザの設定でJavaScriptとCookieが有効になっている必要があります。
* **インターネット接続環境**
    * 安定したインターネット接続が必要で、推奨帯域幅は最低5Mbps以上です。
    * HTTPSによる安全な通信が可能であること。

**本ガイドは、[4.ネットワーク設定とインスタンス作成](https://docs.nhncloud.com/ja/quickstarts/ja/network-setup/)以降の段階から始めます。**

> 今回の学習モジュールでは、4つのシナリオを通して様々なセキュリティ設定方法を学習します。

## シナリオ1.ウェブサーバーインスタンスにセキュリティルールを適用し、外部からのアクセスを許可する

1. NHN Cloudコンソール上部のメニューから実習に使用する組織`(MyORG)`、プロジェクト(`MyPRJ)`、そして`韓国(平村)リージョンを`選択します。
2. 左側のメニューから**Network - Floating IP を**クリックします。
3. フローティングIPリソースのリストのうち、接続されたデバイスが`linux-server-basicである`IPアドレスを**コピーして** **記録します。**
4. ウェブブラウザで新しいウィンドウを開き、`http://복사한 linux-server-basic フローティングIPアドレスを`入力して接続を確認します。
> [参考]
> 
> **そのウェブサーバーに接続すると、ウェブページが表示されません。**これは、そのウェブサーバーのネットワークインターフェースにセキュリティグループを設定していないため、すべての通信が遮断された状態であるためです。
5. ウェブブラウザでコンソールウィンドウに戻った後、左側のメニューから**Network - Security Groupsを**クリックします。
6. **Security Groups**画面で**+ セキュリティグループを作成を**クリックします。
7. **セキュリティグループ作成**ウィンドウで以下の情報を設定し、[**OK**]をクリックします。
    * 名前:`MySG-HTTP`
    * セキュリティルールの追加に\*\*+**をクリックして、次の設定のセキュリティルールを追加します。
        * 方向:`受信`
        * IPプロトコル:`HTTP`
        * Ether: `IPv4`
        * リモート:`CIDR - 0.0.0.0/0/0`
8. 成功]ウィンドウで[**OK**]をクリックします。
9. 左メニューの**Compute - Instanceを**クリックします。
10. **Instance**画面で`linux-server-basic`インスタンスをクリックして選択します。
11. 下部の分割画面で「**ネットワーク**」タブをクリックします。
12. インスタンス作成時に一緒に\*\*自動的に生成されたネットワークインターフェイスリソース(ネットワークインターフェイスIDは任意のIDで指定)**を選択します。
13. ネットワークインタフェースのリストのすぐ上にある[**セキュリティグループの変更]を**クリックします。
14. **セキュリティグループ変更**ウィンドウで、セキュリティグループ選択リストの中から`MySG-HTTPを`選択して追加し、[**OK**]`を`クリックします。
15. ウェブブラウザで新しいウィンドウを開いて`http://복사한 linux-server-basic フローティングIPアドレスを`入力して接続を確認します。
16. Webページが正常に出力されることを確認します。
<br></br>
![pic1](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EB%B3%B4%EC%95%88%20%EC%84%A4%EC%A0%95_%EC%9E%91%EC%97%851%20-%20%EB%B3%B5%EC%82%AC%EB%B3%B8.png)

## シナリオ2.ウェブサーバーインスタンスに許可されたIPのみSSH、ICMP(Pingなど)通信を可能にする

1. 左側のメニューから**Network - Security Groupsを**クリックします。
2. **Security Groups**画面で`MySG-SSHを`クリックして選択します。
3. 下部の[分割画面**セキュリティルール**]タブで、ポート範囲が\*\*22(SSH)**のルールを選択した後、右側の[**変更**]をクリックします。
4. **セキュリティルール変更**ウィンドウで、リモート項目に`CIDRを`クリックし、ドロップダウンメニューから`マイIPを`選択し、[**OK**]をクリックします。
5. 成功]ウィンドウで[**OK**]をクリックします。
6. **+ セキュリティルールの作成を**クリックします。
7. **セキュリティルール作成**ウィンドウで以下の情報を設定し、[**OK**]をクリックします。
    * 方向:`受信`
    * IPプロトコル:`ALL ICMP`
    * Ether: `IPv4`
    * リモート:`私のIP`
8. 成功]ウィンドウで[**OK**]をクリックします。
9. 新しい**ターミナル**または**PowerShellを**実行します。
10. 以下のコマンドで通信テストを行います。

```
#bash
ping (linux-server-basic フローティングIPアドレス)
```
Ping通信が許可されていることを確認します。

## シナリオ3.Network ACLの設定で特定のネットワーク帯域をブロックする 

1. コンソールウィンドウの左側のメニューから**Network - Network ACLを**クリックします。
2. **Network ACL > 管理画面で** **+ Network ACL作成を**クリックします。
3. **Network ACL作成**ウィンドウで以下の情報を設定した後、[**OK**]をクリックします。
    * 名前:`MyACL`
4. 成功]ウィンドウで[**OK**]をクリックします。
5. 生成された`MyACLを`クリックして選択します。
6. 下部の分割画面で**ACL Rule**タブをクリックします。
7. MyACLのACL Ruleリストで、説明に\*\*"default allow rule"\*\*が表示された**101**順のACL Ruleを選択した後、**ACL Rule削除を**クリックします。
8. 成功]ウィンドウで[**OK**]をクリックします。
9. 画面上部の「**Network ACL」タブの「バインディング**」タブをクリックします。
10. **+ ACLバインディングの**作成をクリックします。
11. ACLバインディング作成ウィンドウで以下の情報を**設定し、**[OK]をクリックします。
    * Network ACL: `MyACL`
    * VPC: `MyVPC`
12. 成功]ウィンドウで[**OK**]をクリックします。
13. **ターミナル**または**PowerShellで**下記のコマンドを実行して通信テストを行います。

```bash
ping (linux-server-basic フローティングIPアドレス)
```

* Ping通信がブロックされていることを確認します。

* ウェブブラウザで新しいウィンドウを開いて`http://복사한 linux-server-basic フローティングIPアドレスを`入力して接続を確認します。
* Http通信がブロックされていることを確認します。


## シナリオ4.Network ACL Ruleを追加適用し、外部アクセスを許可する

1. コンソールウィンドウの左側のメニューから**Network - Network ACLを**クリックします。
2. `MyACLを`クリックした後、下部の分割ウィンドウで**ACL Rule**タブをクリックします。
3. **+ ACL Ruleの作成を**クリックします。
4. **ACL Rule作成**ウィンドウで以下の情報を設定した後、[**OK**]をクリックします。
    * IPプロトコル:`ICMP`
    * 出発地CIDR:`0.0.0.0.0/0`
    * 目的地 CIDR:`linux-server-basic フローティングIPアドレス/32`
    * 順序：`110`
    * 適用方法:`遮断`
5. 成功]ウィンドウで[**OK**]をクリックします。
6. **+ ACL Ruleの作成を**クリックします。
7. **ACL Rule作成**ウィンドウで以下の情報を設定した後、[**OK**]をクリックします。
    * IPプロトコル:`ICMP`
    * 出発地 CIDR:`作業しているコンピュータのIPアドレス/ 32`
        > [参考]参考
        >
        > * 作業しているコンピュータのIPアドレスの確認方法
        >     * **ターミナル**または**PowerShellを**実行した後、`curl ifconfig.meを`実行すると、**自分が作業しているコンピュータのIPアドレスを**確認することができます。
    * 目的地 CIDR:`linux-server-basic フローティングIPアドレス/32`
    * 注文する：`109`
    * 適用方法:`許容`
8. **ACL Rule作成**ウィンドウで以下の情報を設定した後、[**OK**]をクリックします。
    * IPプロトコル:`全体適用`
    * 出発地CIDR:`0.0.0.0.0/0`
    * 目的地CIDR:`0.0.0.0.0/0`
    * 注文番号：`32764`
    * 適用方法:`許容`
9. **ターミナル**または**PowerShellで**下記のコマンドを実行して通信テストを行います。

```
#bash
ping (linux-server-basic フローティングIPアドレス)
```

* 作業しているコンピュータのIPのみPing通信が許可されていることを確認します。

* ウェブブラウザで新しいウィンドウを開いて`http://복사한 linux-server-basic フローティングIPアドレスを`入力して接続を確認します。
* すべてのIPでHttp通信が許可されていることを確認します。
<br></br>
![pic2](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/content_image/%EB%B3%B4%EC%95%88%20%EC%84%A4%EC%A0%95_%EC%9E%91%EC%97%854.png)

## 参考資料

* [セキュリティグループ](https://docs.nhncloud.com/ja/Network/Security%20Groups/ja/overview/)
* [Network ACL](https://docs.nhncloud.com/ja/Network/Network%20ACL/ja/overview/)
* [Network Port](https://en.wikipedia.org/wiki/Port_(computer_networking))
* [ACL](https://en.wikipedia.org/wiki/Access-control_list)
* [Whitelist](https://en.wikipedia.org/wiki/Whitelist)
* [Blacklist](https://en.wikipedia.org/wiki/Blacklist_(computing))
* [TCP](https://en.wikipedia.org/wiki/TCP)
* [ICMP(Internet Control Message Protocol)](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol)
* [ping](https://en.wikipedia.org/wiki/Ping_(networking_utility))

## 前の段階

* [4. ネットワーク設定とインスタンス作成](https://docs.nhncloud.com/ja/quickstarts/ja/network-setup/)

## 次のステップ

* [6. データベースの作成と接続](https://docs.nhncloud.com/ja/quickstarts/ja/create-database/)
