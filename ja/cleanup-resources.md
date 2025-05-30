# リソースの整理と削除
**Quickstarts > 12.リソースの整理と削除**

今回の学習モジュールでは、NHN Cloudで未使用のリソースとプロジェクトおよび組織を削除する方法について説明します。これにより、不必要な費用の発生を防ぎ、クラウド環境を最適化することができます。
![mod_info](https://kr1-api-object-storage.nhncloudservice.com/v1/AUTH_2acdfabf4efe4efc8a04c00b348110c9/cdn_origin/prod_cloud_quickstarts/module_info/%EB%A6%AC%EC%86%8C%EC%8A%A4%20%EC%A0%95%EB%A6%AC%20%EB%B0%8F%20%EC%82%AD%EC%A0%9C.png)
## 学習目標

今回の学習モジュールで学ぶ内容は以下の通りです。

* **リソース削除前の確認事項**
    * リソースを削除する前に、依存関係を確認する方法
* **未使用のリソースを削除する**
    * 使用していないサーバー(Compute)、ストレージ(Volume, Object Storage)、データベース(DB)などを削除します。
* **プロジェクトの整理・削除**
    * プロジェクトを削除する手順
* **組織削除**
    * 組織を削除する手順

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

**本ガイドは、[11.コスト管理](https://docs.nhncloud.com/ja/quickstarts/ja/cost-management/)以降の段階から始まります。**

## 使用中のすべてのリソースを削除する 

### ステップ1.組織で使用しているリソースを確認する

> Resource Watcherを使用すると、`MyORG`組織が使用しているすべてのリソースを一目で確認することができます。

1. NHN Cloudコンソール上部のメニューから実習に使用する組織`(MyORG)`をクリックします。
2. `MyORG`組織ダッシュボード内の**組織サービス利用状況から** **Resource Watcherを**クリックします。
3. Resource Watcher画面のタブメニューのうち、[**リソース**]タブをクリックします。
4. **リソースの状態**項目に**全体と**表記されたドロップダウンメニューをクリックし、`使用中の`チェックボックスを選択します`。`
5. **検索を**クリックします。
6. 検索結果画面で現在使用しているリソースを確認します。

### ステップ2.デフォルトのインスタンスリソースを削除します。

> ステップ2～8は、これまで使用したリソースを削除する方法を説明します。

1. NHN Cloudコンソール上部のメニューから`MyPRJを`クリックし、`韓国(平村)`リージョンを選択します。
2. コンソールウィンドウの左側のメニューから**Compute - Instanceを**クリックします。
3. インスタンスリストから`linux-server-basic`インスタンスをクリックして選択します。
4. リストの上にある**---**ボタンをクリックし、**インスタンスの削除**をクリックします。
5. **インスタンス削除**ウィンドウで下記のように設定後、[**OK**]をクリックします。
    * 削除するリソース
        * 追加ブロックストレージ: ✔`(チェック)`
        * フローティングIP: ✔`(チェック)`
    > [参考]参考
    >
    > * 削除するリソースのチェック時
    >     * 該当リソースをチェックしてから削除すると、インスタンス作成時に一緒に作成した対象リソースも一緒に削除されます。
6. 成功]ウィンドウで[**OK**]をクリックします。
7. インスタンスリストから`mysql-db-basic`インスタンスを選択します。
8. リストの上にある**---**をクリックし、**インスタンスの削除を**クリックします。
9. **インスタンス削除**ウィンドウで下記のように設定後、[**OK**]をクリックします。
    * 削除するリソース
        * 追加ブロックストレージ: ✔`(チェック)`
        * フローティングIP: ✔`(チェック)`
10. 成功]ウィンドウで[**OK**]をクリックします。

!!! tip "知っておこう"
    * 追加ブロックストレージ、フローティングIPの削除
        * オブジェクトストレージに保存された資料を削除する場合、ユーザーが内容を確認できるように**削除確認手続きを**行います。

### ステップ3.Auto Scaleリソースを削除する

1. コンソールウィンドウの左側のメニューから**Compute - Auto Scaleを**クリックします。
2. Auto Scaleグループリストから`MyASGroup`スケーリンググループを選択します。
3. リストの上にある**スケーリンググループの削除を**クリックします。
4. **スケーリンググループの削除**ウィンドウで[**OK]**をクリックします。
5. 成功]ウィンドウで[**OK**]をクリックします。

### ステップ4.Imageリソースを削除する

1. コンソールウィンドウの左側のメニューから**Compute - Imageを**クリックします。
2. イメージリストから`linux-server-basic-image`イメージを選択します。
3. **画像の削除を**クリックします。
4. **画像削除**ウィンドウで[**OK**]をクリックします。
5. 成功]ウィンドウで[**OK**]をクリックします。

### ステップ5.Object Storageリソースの削除

1. コンソールウィンドウの左側のメニューから**Storage - Object Storageを**クリックします。
2. オブジェクトストレージリストで左側の`myobsの`チェックボックスを選択します。
3. **コンテナを空にするを**クリックします。
4. "コンテナを空にするには、選択したコンテナ名を入力してください。" 下部の入力欄に`myobsを`入力し、[**OK**]をクリックします。
5. 成功]ウィンドウで[**OK**]をクリックします。
6. `myobsの`オブジェクト数が`0になって`いることを確認します。
7. `myobs`チェックボックスを選択し、**コンテナの削除を**クリックします。
8. コンテナの削除ウィンドウで[**OK**]をクリックします。
9. 成功]ウィンドウで[**OK**]をクリックします。

!!! tip "知っておこう"
    * オブジェクトストレージの削除確認
        * オブジェクトストレージに保存された資料を削除する場合、ユーザーが内容を確認できるように**削除確認手続きを**行います。

### ステップ6.ネットワークリソースの削除

1. コンソールウィンドウの左側のメニューのうち、**Network - Load Balancerを**クリックします。
2. ロードバランサー一覧から`MyLBを`選択します`。`
3. **ロードバランサーの削除を**クリックします。
4. **ロードバランサーの削除**ウィンドウで[**OK**]をクリックします。
5. 成功]ウィンドウで[**OK**]をクリックします。
6. コンソールウィンドウの左側のメニューのうち、**Network - Floating IPを**クリックします。
7. フローティングIPリストから**保有しているすべてのフローティングIPの**チェックボックスを選択します。
8. **フローティングIPの削除を**クリックします。
9. フローティングIPの削除ウィンドウで[**OK]**をクリックします。
10. コンソールウィンドウの左側のメニューから**Network - Security Groupsを**クリックします。
11. セキュリティグループ一覧から`MySG-SSH、MySG-HTTP、MySG-DBの`チェックボックスを選択します。
12. **セキュリティグループの削除を**クリックします。
13. **セキュリティグループ削除**ウィンドウで「**OK」を**クリックします。
14. 成功]ウィンドウで[**OK**]をクリックします。
    > [参考]参考
    >   * default セキュリティグループ
    >       * defaultセキュリティグループは、NHN Cloudで使用するデフォルトのセキュリティグループで、セキュリティグループ名を変更したり、削除することはできません。 ただし、デフォルトのセキュリティグループ内のセキュリティルールは作成したり、削除することができます。
15. コンソールウィンドウの左側のメニューのうち、**Network - VPCを**クリックします。
16. VPCリストから`MyVPCを`クリックして選択します。
17. **VPC削除を**クリックします。
18. **VPC削除**ウィンドウで[**OK**]をクリックします。
19. 成功]ウィンドウで[**OK**]をクリックします。

!!! tip "知っておくべきこと"
    * VPC削除依存サービス
        * VPCを削除すると、VPC内に作成したサブネット`(MySubnet)`、ルーティングテーブル(`MyRT)`、インターネットゲートウェイなどのリソースも一緒に削除されます。


### ステップ7.モニタリングリソースの削除

1. コンソールウィンドウの左側のメニューから**Monitoring - Cloud Monitoringを**クリックします。
2. **ダッシュボード**タブ内の`MyDashboard`詳細タブで右側の編集モードのトグルスイッチをクリックして有効にします。
3. `MyDashboard`内の全てのウィジェット(Instance-Basic、Instance-Disk-Basic、Instance-Network-Basicなど)の右上に**ある⋮を**選択し、**削除を**クリックします。
4. 通知ウィンドウで[**OK**]をクリックします。
5. `MyDashboard` **の⋮を**クリックし、**ダッシュボードの削除を**クリックします。
6. ダッシュボード削除進行確認通知ウィンドウで「**OK」を**クリックします。
7. ダッシュボードの削除完了通知ウィンドウで[**OK**]をクリックします。
8. **ダッシュボード**タブの右側の**通知管理**タブをクリックします。
9. **通知設定**詳細タブ内の通知リストのうち、`MyAlarmの`チェックボックスを選択します。
10. **通知の削除を**クリックします。
11. 通知削除の進行を確認し、通知ウィンドウで[**OK**]をクリックします。

### ステップ8.キーフェアリソースを削除する

1. コンソールウィンドウの左側のメニューから**Compute - Key Pairを**クリックします。
2. キーペア一覧から`MyKeyの`チェックボックスを選択します。
3. **キーペア削除を**クリックします。
4. **キーペア削除**ウィンドウで[**OK]**をクリックします。
5. **成功**]ウィンドウで[**OK**]をクリックします。

!!! tip "知っておくべきこと"
    * キーフェアの適用範囲
        * キーペアはプロジェクト単位ではなく、**ユーザーのアカウント単位で**管理されます。他のプロジェクトでキーペアを引き続き使用したい場合は、そのキーを削除する必要はありません。

### ステップ9.サービスを無効にし、プロジェクトを削除します。

1. NHN Cloudコンソール上部にある`プロジェクト` **タブで**MyPRJをクリックします。
2. プロジェクトで[**プロジェクト管理**]タブをクリックします。
3. **プロジェクト管理**画面内の**利用中のサービス**項目で、**基本インフラストラクチャ**グループに属するサービス(Instance, Key Pairなど)の右側にある[**無効化]を**クリックします。
4. 基本インフラストラクチャサービス無効化ウィンドウで、以下の2つの事項を確認した後、チェックボックスを選択し、[**OK**]をクリックします。
    * 上記の内容を全て確認し、サービス非アクティブ化時の情報削除に同意します。
    * すべての基本インフラサービス(Instance,Key Pairなど)
5. **サービス無効化通知**ウィンドウで「**OK」を**クリックします。
6. **利用中のサービス**項目で、**Object Storage**サービスの右側にある[**無効化]を**クリックします。
7. 基本インフラストラクチャサービス無効化ウィンドウで以下の内容を確認し、チェックボックスを選択した後、[**OK**]をクリックします。
    * 上記の内容を全て確認し、サービス非アクティブ化時の情報削除に同意します。
8. **サービス無効化通知**ウィンドウで「**OK」を**クリックします。
9. **プロジェクト削除の**項目で**プロジェクトの削除を**クリックします。
10. **プロジェクト削除確認**ウィンドウで[**OK**]をクリックします。
11. **プロジェクト削除成功**通知ウィンドウで[**OK**]をクリックします。

### ステップ10.組織を削除する

1. NHN Cloudコンソール上部にある**組織**タブの`MyORGを`クリックします。
2. 組織ダッシュボード画面で**組織管理**タブをクリックします。
3. **組織の削除の**項目で**組織の削除を**クリックします。
4. **組織削除確認**ウィンドウで[**OK**]をクリックします。
5. **組織削除成功**ウィンドウで[**OK**]をクリックします。
<br></br>
> おめでとうございます。すべてのクイックスタートを終えました。</font></p>

<br></br>

## 参考資料

* [インスタンスの状態変更(削除)](https://docs.nhncloud.com/ja/Compute/Instance/ja/console-guide/#_11)
* [サブネット削除](https://docs.nhncloud.com/ja/Network/VPC/ja/console-guide/#_10)
* [VPC](https://docs.nhncloud.com/ja/Network/VPC/ja/console-guide/#vpc)
* [ブロックストレージの削除](https://docs.nhncloud.com/ja/Storage/Block%20Storage/ja/console-guide/#_3)
* [オブジェクトストレージコンテナの削除](https://docs.nhncloud.com/ja/Storage/Object%20Storage/ja/console-guide/#_7)

## 前の段階

* [11.コスト管理](https://docs.nhncloud.com/ja/quickstarts/ja/cost-management/)