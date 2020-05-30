# AWSでサービスを作ってみよう!
- keywords: Route53, EC2, S3, ALB

## この発表について
### 対象者
  - AWSに興味がある人
  - AWSを使ってサービスを作ってみたい人

### ゴール
- AWSのサービス群をだいたい把握
- EC2で"HelloWorld"を表示

## AWSのサービス群
![aws services](https://github.com/coworker-jp/aws-sample/blob/master/step1/img/services.png?raw=true)

### それぞれ一行で言うと
```
◆コンピューティング
・EC2：仮想的なプライベートサーバー
・Lightsail：Amazonが提供するホスティングプロバイダ(VPS、DNS、ストレージ)
・Lambda：PythonやNode.js、Goなどで書かれたコードを実行できる。並列実行も可。
・Batch：EC2マシンでソフトウェアジョブを回せる
・Elastic Beanstalk：マネージドな仮想マシンでソフトウェアを実行できる
・Serverless Application Repository：(Lambdaに)デプロイ可能なサーバーレスアプリケーション用リポジトリ
・Outposts：自分のデータセンターでAmazonのサービスを利用できる
・Image Builder：EC2のイメージを自動で作成する

◆ストレージ
・S3：ファイルストレージ。直接マウントはできないがHTTP経由でファイルをダウンロードできる。
・EFS：NFS。ネットワーク上のディスクを自分のマシンにマウントできる。
・FSx：EC2マシンから接続できるWindowsかLustreのファイルシステム
・S3 Glacier：バックアップやアーカイブのための低コストストレージシステム
・Storage Gateway：iSCSIなどを使ってS3を自分のマシンに接続できる
・Backup：異なるAWSのサービス(EC2やRDSなど)のバックアップを自動で行ってくれる

◆データベース
・RDS：マネージドなMySQL、PostgreSQLデータベース
・DynamoDB：大規模＆スケーラブルな非リレーショナルデータベース
・ElastiCache：マネージドなMemcacheおよびRedisマシン
・Neptune：グラフデータベース
・Redshift：データウェアハウス。ストリームで処理できるデータを大量に保存できる。
・QLDB：イミュータブルかつ暗号で検証されるデータ(マネートランザクションなど)用のデータベース
・DocumentDB：MongoDBのクローン(完全な互換性はない)
・Keyspaces：マネージドなApache Cassandraのクローン

◆マイグレーションとデータ転送
・Migration Hub：データセンターからAWSへのマイグレーションを行う
・Application Discovery Service：データセンターのサービス情報を収集
・Database Migration Service：オンラインのままデータベースをRDSへマイグレーションできる。異なるデータ構造間でも可。
・Transfer Family：S3を基盤とする(S)FTPサービス。FTPでS3バケットにデータを転送できる。
・Snowball：AWSのマシンを取得してデータセンターに接続し、AWSにデータを高速に転送してくれる一連のサービス
・DataSync：データセンターとAWS間でデータを同期する

◆ネットワークとコンテンツデリバリー
・VPC：AWS内でVPNを使える
・CloudFront：コンテンツデリバリネットワーク
・Route 53：ドメインネームとレコードを管理する
・API Gateway：HTTPのAPIを作成し、それらを異なるバックエンドに接続する
・Direct Connect：自分のシステム(もしくはデータセンター)とAWS間を(物理的に)接続する
・App Mesh：ECSやEKSのコンテナのサイドカーとしてEnvoyを自動的に実行する
・Cloud Map：コンテナのためのサービスを検出する
・Global Accelerator：アプリケーションをエッジロケーションで実行する。CDNのアプリケーション版

◆開発ツール
・CodeStar：テンプレートのコードやCodeCommit、CodeBuildのテンプレを使ってアプリを迅速に開発できる
・CodeCommit：Amazonのソースリポジトリ(gitのレポジトリなど)
・CodeBuild：CIサービス
・CodeDeploy：デプロイメントサービス
・CodePipeline：ワークフローを定義してCDを行う
・Cloud9：オンラインのIDE
・X-Ray：アプリケーションを分析したりデバッグしたりできる。PythonやNode.js、Goなどをサポート。

◆ロボティクス
・RoboMaker：ロボットエンジニアのためのクラウドソリューション。シミュレーションやテスト、ロボット用アプリの安全なデプロイができる。

◆顧客ごとの最適化
・IQ：必要にあわせてAWSの専門家を雇える
・サポート：AWSのサポートセンター
・Managed Services：AWSにAWSサービスの管理をお任せする

◆ブロックチェーン
・Managed Blockchain：ブロックチェーン

◆衛星
・Ground Station：衛星通信サービス

◆量子技術
・Braket：なんらかの量子技術。記事作成時点ではプレビュー段階でよくわからないとのこと。

◆マネジメントとガバナンス
・Organizations：複数の組織やアカウントの設定
・CloudWatch：さまざまなAWSの要素からログを取得する
・Auto Scaling：自分で設定した入力やルールに基づいてリソースをスケールする
・CloudFormation：テンプレートを使ってAWSの要素を作ったり設定したりできる
・CloudTrail：AWSサービス内で誰が何をしたか記録する
・Config：AWSリソースの設定を監査する
・OpsWorks：ChefやAnsibleによる自動化
・Service Catalog：クラウド内のアイテムやコードなどのリストを管理
・Systems Manager：アプリごとなど、自由にリソースをグループ化し、データを見ることができる
・AppConfig：アプリケーションの設定データを保存したり公開したりできる
・Trusted Advisor：アカウントについてのコストやセキュリティなどの問題のチェックをしてくれる
・Control Tower：複数のアカウントの管理
・License Manager：ライセンスの管理
・Well-Architected Tool：システムについてのアンケートを作成し、ベストプラクティスに沿っているか確認できる
・Personal Health Dashboard：AWSのステータスページ
・Chatbot：AWSをSlackと連携できる
・Launch Wizard：MS SQLやSAPのソフトウェアをデプロイする
・Compute Optimizer：最適なリソースを発見し、コスト削減の方法を指示してくれる

◆メディアサービス
・Elastic Transcoder：S3のファイルを異なるフォーマットに変換したり、S3に戻したりする
・Kinesis Video Streams：メディアストリームをキャプチャする
・Elemental MediaConnect：不明とのこと
・Elemental MediaConvert：メディアを異なるフォーマットに変換する
・Elemental MediaLive：ライブビデオを不特定多数に共有する
・Elemental MediaPackage：不明とのこと
・Elemental MediaStore：不明とのこと
・Elemental MediaTailor：動画のブロードキャストに広告を挿入できる
・Elemental アプライアンスとソフトウェア：オンプレミスでビデオを作成できる。基本的には上記のサービスを組み合わせたもの。費用がかかりそう。

◆機械学習
・SageMaker：機械学習ツール
・CodeGuru：機械学習でJavaのコードをプロファイルする
・Comprehend：機械学習によってメールやツイートの内容を機械に理解させ、分類させる
・Forecast：データから予測する
・Fraud Detector：プレビュー段階でわからない
・Kendra：質問による検索サービス
・Lex：音声会話やチャットボットを作成できる
・Machine Learning：非推奨。SageMakerが後継。
・Personalize：データに基づいて個人に最適化されたレコメンデーションを作成できる(Mahout？)
・Polly：テキストから音声へ異なる言語で変換できる
・Rekognition：画像中の物体や人物を認識する
・Textract：画像中のテキストを認識してテキストとして出力する(光学文字認識)
・Transcribe：音声をテキストに変換する
・Translate：テキストを他言語に翻訳する
・DeepLens：機械学習を行うビデオカメラ
・DeepRacer：機械学習でレース車両をプログラムし競い合うゲームの一種
・Augmented AI：機械学習をより良くするために人間を学習のフローに参加させる
・DeepComposer：コンピューターが作曲する。文字通りものすごいもの。

◆分析
・Athena：クエリデータをS3バケットに保存する
・EMR：ビッグデータフレームワーク実行してスケールできる
・CloudSearch：マネージドなドキュメント検索システム(ElasticsearchのAWS版)
・Elasticsearch Service：SaaSのElasticsearch
・Kinesis：分析可能なかたちで膨大なデータを収集する(ELKスタックのようなもの？)
・QuickSight：ビジネスインテリジェンスサービス
・Data Pipeline：DynamoDBやRDS、S3にデータを移動させたり変形させたりできる
・Data Exchange：目的のデータを利用できるAPIを探してくれる、高額になる可能性あり
・Glue：ETLサービス、データの品質を上げ、検証してくれる
・Lake Formation：データレイクを作成する
・MSK：SaaSのApache Kafka

◆セキュリティと識別、コンプライアンス
・IAM：ユーザーやAWSサービスを管理できるAWSの権限システム
・Resource Access Manager：Route 53やEC2などのAWSリソースを他のアカウントと共有できる
・Cognito：ユーザーとパスワード管理システム。アプリケーションのユーザー管理に便利。
・Secrets Manager：キーなどの暗号化データを保護してくれる。シークレットの自動ローテートも可能。
・GuardDuty：CloudTrailやVPCのログを自動でスキャンし脅威に備える
・Inspector：ネットワークやマシンの(セキュリティ上の)問題を自動で検出してくれる
・Macie：S3バケット内のデータを分析し、個人情報をチェックしてくれる
・Single Sign-On：アプリケーションでシングルサインオン機能を利用できる
・Certificate Manager：SSL証明書の管理や(無料の)証明書を発行できる
・Key Management Service：暗号化キーの管理
・CloudHSM：ハードウェアセキュリティモジュール。暗号鍵の生成と操作が可能。
・Directory Service：SaaSのActive Directory
・WAFとShield：ウェブアプリケーションファイアウォール。ルールを設定したり事前に用意したルールを指定したりできる。
・Firewall Manager：組織内の異なるアカウントのファイアウォール管理
・Artifact：クラウドコンプライアンスのドキュメント(ISO/IEC 27001のようなもの)
・Security Hub：GuardDutyやInspector、Macieなどを利用した総合的なセキュリティチェッカー
・Detective：(Security Hubなどから)セキュリティ上の問題をログに残す

◆モバイル
・Amplify：AWSでフロントエンド＆バックエンドアプリを自動生成して自動デプロイさせる
・Mobile Hub：今はAmplifyの一部に
・AppSync：接続可能なバックエンドのAPIを作成できる。Amplify経由でも作成可能。
・Device Farm：AWSのBrowserStack。異なるモバイル端末やブラウザでのテストを自動で行える。

◆ARとVR
・Sumerian：わからない。テイセン氏の環境ではブラウザがクラッシュしたとのこと。

◆アプリケーション統合
・Step Functions：Amazon独自の言語でマシン構成を記述できる
・AppFlow：自動で複数のアプリを連携できる(Zapierみたいなもの？)
・EventBridge：イベントバスシステムのようなもの
・MQ：Amazonが管理するApache ActiveMQ
・Simple Notification Service：メールやSMSなどでシステムに関する通知ができる
・Simple Queue Service：メッセージキューシステム
・SWF：ワークフローを作成できる

◆AWSのコスト管理
・Cost Explorer：AWSの費用状況を可視化
・Budgets：AWSの予算を作成
・Marketplace：ソフトウェアインストール済みのAMIを検索し購入できる

◆カスタマーエンゲージメント
・Connect：AWSのコールセンタープラットフォーム
・Pinpoint：テンプレートに沿ったメールやSMS、電話による応答ができる
・Simple Email Service：メール送信ができる。メールプロバイダー。

◆ビジネスアプリケーション
・Alexa for Business：ビジネスとAlexaを結びつける
・Chime：ZoomのAWS版
・WorkMail：GmailやGoogleカレンダーのAWS版

◆エンドユーザーコンピューティング
・WorkSpaces：WindowsもしくはLinuxの仮想デスクトップサービス
・AppStream 2.0：アプリケーションをブラウザに配信できる
・WorkDocs：オンラインに文書を保存し管理できる
・WorkLink：イントラネットにモバイル端末で接続できる

◆IoT
・IoT Core：MQTTブローカーを通してIoTデバイス群を管理する
・FreeRTOS：自動でIoT CoreやGreengrassに接続できるマイコン用RTOS
・IoT 1-Click：Lambdaなどのシステムと1クリックで接続し管理できる
・IoT Analytics：分析のためにさまざまなメッセージを構造化して保存できる
・IoT Device Defender：デバイスの異常を検知しアクションを起こす
・IoT Device Management：IoTデバイスをグループ化し、ジョブのスケジュールやリモートアクセスの設定ができる
・IoT Events：デバイスの利用状況を監視し、AWSサービスやジョブをデバイス自ら実行できる
・IoT Greengrass：IoT Coreへの接続が断続的な場合、メッセージブローカーはローカルで相互通信可能な200台までのデバイスにメッセージをバッファできる
・IoT SiteWise：産業機器からのデータを収集、構造化、分析、可視化する
・IoT Things Graph：IoTデバイスがAWSサービスとどう通信しているかを可視化できるCloudFormationのような構築サービス

◆ゲーム開発
・GameLift：ゲームサーバーを低遅延でAWSにデプロイする

◆コンテナ
・Elastic Container Registry：Docker HubのようにDockerイメージを保存できる
・Elastic Container Service：EC2やFargate上でコンテナを起動できる
・Elastic Kubernetes Service：SaaSのKubernetes

```

- [参考: AWSの膨大で複雑なサービス群をすべて「たった1行」で説明していくとこうなる](https://gigazine.net/news/20200528-aws-one-line-explanation/)


## EC2を立ててみる
- VPC, subnetまわりの設定
  - 前のyorisiloさんの勉強会参考になるよ
- EC2のスペックはいろいろ
  - [インスタンスタイプ](https://aws.amazon.com/jp/ec2/instance-types/)
- セキュリティグループを設定しよう
  - どのportあける？どう制限する？
  - セキュリティに気をつけてね!
- key pairを保存
- ElasticIPを使って,IPを固定
  - EC2のIPはそのままだと固定されない
- sshしてみる
    - [SSH クライアントを使用して Linux インスタンスに接続する](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)

``` ssh
ssh -i /path/my-key-pair.pem ec2-user@ec2-198-51-100-1.compute-1.amazonaws.com
```

## Service公開をしてみる
```
# yum を update
sudo yum update -y

# apache install
sudo yum -y install httpd24

# 該当file作成
cd /var/www/html
sudo vim index.html

sudo service httpd start
```

※ /etc/httpd/conf/httpd.conf に設定を書く
※ nginxでもいい

- IPにアクセス
  - EC2で"HelloWorld"が表示される
![helloworld](https://github.com/coworker-jp/aws-sample/blob/master/step1/img/helloworkd.png?raw=true)


## 本番運用にむけて ~ 
- VPC, subnetをしっかり構築
- Route53(DNS)でドメインを紐付けて引き当てられるようにしよう
- RDSなど使って、DBをつなぐ
- ALBにつなげて、Load Balancing
- キャッシュさせたかったら, 前面にCloudFrontを置こう


