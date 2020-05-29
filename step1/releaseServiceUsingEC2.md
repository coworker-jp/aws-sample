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
- ![aws services](https://github.com/coworker-jp/aws-sample/blob/master/step1/img/services.png?raw=true)
- Route53
- EC2
- RDS
- 
...


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

``` ssh
ssh -i /path/my-key-pair.pem ec2-user@ec2-198-51-100-1.compute-1.amazonaws.com
```
  - [SSH クライアントを使用して Linux インスタンスに接続する](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html)

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
* nginxでもいい

- IPにアクセス
  - EC2で"HelloWorld"が表示される
  - ![helloworld](https://github.com/coworker-jp/aws-sample/blob/master/step1/img/helloworkd.png?raw=true)
