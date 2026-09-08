<!-- pre-align:aligned sig=b5f49994b6df -->

# モニタリング設定
**Quickstarts > 8.モニタリング設定**

今回の学習モジュールでは、NHN Cloudコンソールで提供するMonitoringサービスについて詳しく説明し、直接実習してみます。NHN CloudのCloud Monitoringサービスは、クラウド環境で運営されるインフラとアプリケーションの状態をリアルタイムでモニタリングし、異常兆候を迅速に検知することができます。

![mod_info](https://static.toastoven.net/prod_cloud_quickstarts/module_info/%E1%84%86%E1%85%A9%E1%84%82%E1%85%B5%E1%84%90%E1%85%A5%E1%84%85%E1%85%B5%E1%86%BC%20%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8C%E1%85%A5%E1%86%BC_ja.png)
<a id="learning-objectives"></a>
## 学習目標 { #learning-objectives }

今回の学習モジュールで学ぶ内容は以下の通りです。

* **NHN Cloudのモニタリングコンセプト**
    * NHN Cloudが提供するモニタリングサービスの概要
    * システムパフォーマンス、ネットワークトラフィック、アプリケーションログの監視の重要性
    * リアルタイムの状態追跡と長期的なパフォーマンス分析の違い
* **NHN Cloudモニタリングツール紹介**
    * **Cloud Monitoringの**概要と機能
    * モニタリングダッシュボードの構築
        * NHN Cloudの基本ダッシュボードを活用
        * テンプレートを利用したダッシュボード設定
    * 警告と通知の設定
        * NHN Cloud通知システムを通じた異常状態検出
        * メール、SMSなどの条件別通知設定

<a id="before-you-begin"></a>
## 始める前に { #before-you-begin }

今回の学習モジュールを実践する前に、次のことを行うことをお勧めします。

* **標準のインターネットブラウザ**
    * Google Chrome、Microsoft Edge、Firefox、Safariなどの最新バージョンのブラウザがインストールされている必要があります。
    * ブラウザの設定でJavaScriptとCookieが有効になっている必要があります。
* **インターネット接続環境**
    * 安定したインターネット接続が必要で、推奨帯域幅は最低5Mbps以上です。
    * HTTPSによる安全な通信が可能であること。
* **会員アカウント**
    * 決済手段を登録したNHN Cloudアカウントが必要です。
    * NHN Cloudのホームページにログインする必要があります。

    **本ガイドは、[7.ストレージの作成と設定](https://docs.nhncloud.com/ja/quickstarts/ja/create-storage/)以降の段階から始まります。**

<a id="monitoring-cloud-resources-with-the-cloud-monitoring-service"></a>
## Cloud Monitoringサービスによるクラウドリソースモニタリング { #monitoring-cloud-resources-with-the-cloud-monitoring-service }

<a id="step-1-create-an-instance-detailed-metrics-dashboard-with-cloud-monitoring"></a>
### ステップ1.Cloud Monitoringでインスタンス詳細指標のダッシュボードを作成します。 { #step-1-create-an-instance-detailed-metrics-dashboard-with-cloud-monitoring }

1. NHN Cloudコンソール上部のメニューから実習に使用する組織`(MyORG)`、プロジェクト(`MyPRJ)`、そして`韓国(平村)リージョンを`選択します。
2. コンソールウィンドウの左側のメニューから**Monitoring - Cloud Monitoringを**クリックします。
3. **+ ダッシュボード作成を**クリックします。
4. **ダッシュボード作成**ウィンドウで以下の情報を設定し、[**OK**]をクリックします。
    * ダッシュボード名:`MyDashboard`
5. `MyDashboard`タブの右端にある**+ウィジェットの追加を**クリックします。
6. **ウィジェット追加**画面で以下の情報を設定し、**追加を**クリックします。
    * 基本設定
        * ウィジェット名:`Instance-CPU-Basic`
        * グラフタイプ:`Line`
        * サービス:`Instance`       
    > [参考]参考
    >
    > * サービス変更アラーム
    >    * ウィジェットのサービスが変更されると、設定した内容が削除されることがあります。変更時、通知で確認できるので、既存の作業した内容を事前に追加した後、変更して作業できるようにします。
    * 指標設定
        * リソースタイプ:`CPU`
        * 指標項目：`CPU使用率、CPU平均負荷(5m)`
7. **+ ウィジェットの追加を**クリックしてウィジェットを追加で作成します。
8. **ウィジェット追加**画面で以下の情報を設定し、**追加を**クリックします。
    * 基本設定
        * ウィジェット名:`Instance-Disk-Basic`
        * グラフタイプ:`Stacked Area`
        * サービス:`Instance`
    * 指標設定
        * リソースタイプ:`Disk`
        * 指標項目：`ディスク使用率, マウント別ディスク使用率, デバイス別ディスク読み取り, デバイス別ディスク書き込み`
9. **+ ウィジェットの追加を**クリックしてウィジェットを追加で作成します。
10. **ウィジェット追加**画面で以下の情報を設定し、**追加を**クリックします。
    * 基本設定
        * ウィジェット名:`Instance-Network-Basic`
        * グラフタイプ:`Column`
        * サービス:`Instance`
    * 指標設定
        * リソースタイプ:`Network`
        * 指標項目：`デバイス毎のネットワークデータ送信、デバイス毎のネットワークデータ受信`
11. `MyDashboardに`追加されたウィジェットが正常に見えるか確認します。

<a id="step-2-check-out-your-projects-custom-dashboard"></a>
### ステップ2.プロジェクトカスタムダッシュボードを確認する { #step-2-check-out-your-projects-custom-dashboard }

1. コンソールウィンドウの上部にある`MyProjectという`名前のプロジェクトタブをクリックします。
2. `MyProjectの`メイン画面で`カスタムダッシュボードタブを`クリックします。
3. ステップ1で追加した`MyDashboardの`ウィジェットが正常に見えるか確認します。

<a id="step-3-set-up-email-notifications-when-an-instance-experiences-a-cpu-overload"></a>
### ステップ3.インスタンスにCPU過負荷が発生した場合、電子メールで通知を設定する { #step-3-set-up-email-notifications-when-an-instance-experiences-a-cpu-overload }

1. **Cloud Monitoring**サービス画面で「**通知管理**」タブをクリックします。
2. **+ 通知設定を**クリックします。
3. **通知作成**画面で以下の情報を設定し、**保存を**クリックします。
    * 基本情報
        * 名前:`MyAlarm`
        * サービス:`Instance`
    * 通知設定
        * リソースタイプ:`CPU`
        * 指標:`CPU詳細(user)`
        * フィルター
            * **+追加を**クリックし、以下の設定を入力します。
            * ラベル:`インスタンス`, 演算子:`=,` 条件:`linux-server-basic`
            * 編集:**保存を**クリック
        * 条件
            * 比較方法:`>、し`きい値:`50`、持続時間:`1`分
                * しきい値が50を超えると、通知は1分間続きます。
            * 編集:**保存を**クリック
    * 通知受信対象
        * 通知受信グループ名が`デフォルトの通知受信グループである`対象右側の**追加**チェックボックスを**選択**

    > [参考]参考
    >
    > * 通知受信グループの選択
    >    * その通知受信グループ名の右側の追加チェックボックスを選択すると、すぐ上に選択した通知受信グループが追加されることが確認できます。

4. `MyAlarmが`作成されたことを確認し、**通知の使用可否の**トグルボタンが有効になっていることを確認します。

!!! tip "知っておくべきこと"
    * トグルボタンの状態
        * トグルボタンが有効な状態は、楕円内の白い円が右に移動した状態です。トグルが有効になると、色が青色で表示されます。
        * トグルボタン無効の状態は、楕円内の白い円が左に移動した状態です。トグルボタンが無効の場合、色がグレーで表示されます。


<a id="step-4-check-the-instances-history-of-cpu-overload-events"></a>
### ステップ4.インスタンスのCPU過負荷イベント発生履歴の確認 { #step-4-check-the-instances-history-of-cpu-overload-events }

1. コンソールウィンドウの左側のメニューから**Network - Floating IP**をクリックします。
2. フローティングIPリソースのリストのうち、接続されたデバイスが`linux-server-basicである`IPアドレスを**コピーして** **記録します。**
3. ウェブブラウザで新しいウィンドウを開いて`http://복사한 linux-server-basic フローティングIPアドレスを`入力してウェブページにアクセスします。
4. ウェブページ本文にある**Start Stress Testを**クリックした後、2分間待ちます。この作業は`linux-server-basic`インスタンスのCPU使用量(CPU詳細(user))にランダムに過負荷を発生させます。
5. しばらくして、文字とメールでアラーム発生結果が受信されるかどうかを確認します。
6. **Cloud Monitoring**サービス画面で「**通知管理**」タブをクリックします。
7. **通知管理画面で、** **通知発生履歴**タブをクリックします。
8. 本文内の**検索を**クリックして、通知発生履歴を確認します。

<a id="references"></a>
## 参考資料 { #references }

* [Metric](https://en.wikipedia.org/wiki/Metric_system)
* [Monitoring](https://en.wikipedia.org/wiki/System_monitor)
* [Metric Dictionary](https://docs.nhncloud.com/ja/Monitoring/Cloud%20Monitoring/ja/metric-dictionary/)
* [Cloud Monitoring](https://docs.nhncloud.com/ja/Monitoring/Cloud%20Monitoring/ja/overview/)
* [CloudTrail](https://docs.nhncloud.com/ja/Governance%20&%20Audit/CloudTrail/ja/overview/)
* [Stress testing](https://en.wikipedia.org/wiki/Stress_testing_(computing))

<a id="previous-step"></a>
## 前の段階 { #previous-step }

* [7. ストレージの作成とpublish](https://docs.nhncloud.com/ja/quickstarts/ja/create-storage/)

<a id="next-steps"></a>
## 次のステップ { #next-steps }

* [9. -バックアップと復旧](https://docs.nhncloud.com/ja/quickstarts/ja/backup-restore/)
