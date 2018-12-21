# aws-network-basis
書籍「[Amazon Web Services 基礎からのネットワーク&サーバー構築 改訂版](http://amzn.asia/d/iPlJcog)」の内容を AWS CloudFormation で構築する、a-knowさん発祥のチュートリアル。

起源：[AWS CloudFormation への入門に「Amazon Web Services 基礎からのネットワーク&サーバー構築」を使ってみた - えいのうにっき](https://blog.a-know.me/entry/2017/02/13/222100)

### NOTE
- Chapter.2 ネットワークを構築する
    - [AWS::EC2::VPC - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc.html)
        - CidrBlock required.
        - デフォルト値が入っているものはどうする？ドリフト検知用に指定した方が良い？
        - Tags: Nameは入れておく方が管理画面で見やすい
    - [AWS::EC2::Subnet - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-subnet.html)
        - CidrBlock, VpcId required.
        - VpcId: !Ref MyVPC、論理ID？は具体的に何を見ている？
            - リソースの論理名、パラメーターの論理名
    - [AWS::EC2::InternetGateway - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-internetgateway.html)
        - no param required.
        - VPC に attach して使う
    - [AWS::EC2::VPCGatewayAttachment - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-vpc-gateway-attachment.html)
        - VpcId required.
        - InternetGatewayId or VpnGatewayId required.
    - [AWS::EC2::RouteTable - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-route-table.html)
        - VpcId required.
        - ルートテーブルの作成、ルートを追加して使う
    - [AWS::EC2::Route - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-route.html)
        - DestinationCidrBlock他, GatewayId他, RouteTableId required.
        - ルートテーブルに追加するルート
        - DependsOn属性 指定したリソース作成後に実行する?
    - [AWS::EC2::SubnetRouteTableAssociation - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-subnet-route-table-assoc.html)
        - RouteTableId, SubnetId
        - サブネットをルートテーブルに紐付ける

- Chapter.3 サーバーを構築する
    - [AWS::EC2::SecurityGroup - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html)
        - EC2セキュリティグループをつくる。
        - GroupDescription, VpcId required.
    - [EC2 セキュリティグループルールのプロパティタイプ - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group-rule.html)
        - セキュリティグループのルール概要
        - CidrIp, IpProtocol required.
        - FromPort, ToPortで設定
        - 0.0.0.0/0 全てのIP
        - Egressは設定しなければ全開放
    - [AWS::EC2::Instance - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-instance.html)
        - さすがに設定項目豊富だな
        - KeyName キーペアは既存の物を使わないとか？
            - 事前に作成したものだけ利用可能っぽい
    - [AWS::EC2::NetworkInterface - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-network-interface.html)
        - ネットワーク設定
        - GroupSetは文字列リスト型で指定
    - [Amazon EC2 ブロックデバイスマッピングプロパティ - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-blockdev-mapping.html)
        - ブロックデバイスのマッピング
    - [Amazon Elastic Block Store ブロックデバイスプロパティ - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-blockdev-template.html)
        - ブロックデバイスの詳細設定
        - TagsのName追加はできないらしい
    - [AWS::EC2::EIP - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-eip.html)
        - Elastic IP を作成し紐付け
        - VPCを利用する場合は DependsOn を指定すること
        - 既存のEIPを紐付けるなら [AWS::EC2::EIPAssociation - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-eip-association.html)

- Chapter.4 Web サーバーソフトをインストールする
    - CFnで作業はできるが、適性範囲を超えている気はする。
    - [CloudFormation ヘルパースクリプトリファレンス - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/cfn-helper-scripts-reference.html)
    - [Fn::Sub - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-sub.html)
    - rhel7系では動かないルールがありそうな気配。

- Chapter.5 HTTPの動きを確認する
    - node.jsをインストール Chapter.4のおさらい
        - シェルスクリプト中 sudo 使える。IAM とか権限どうなんだろう？
        - 8080 ポート開ける
    - `app.js`を動かしてみる
    - サンプルコードは6系 `curl -sL https://rpm.nodesource.com/setup_6.x | bash -`
    - そういえば telnet 最近 macOS に入ってない。

- Chapter.6 プライベートサブネットを構築する
    - 起動中のスタックにDB用のEC2と追加でRDSもプライベートサブネットに設置してみる。
    - MyPrivateSubnet のルートテーブルはデフォルトのままでOK。
    - セキュリティグループ「任意の場所」を設定したいときに「0.0.0.0/0, ::/0」はダメ。
        - 0.0.0.0/0 で同じ意味という事か？
    - WEBサーバーからDBサーバーへの疎通確認はping
    - pem アップロードして踏み台アクセス
    - [ ]ssh-agent 経由でアクセス

- Chapter.7 NATを構築する
- Chapter.8 DB を用いたブログシステムの構築

### MEMO
- yamlファイルどこに置いておくのが良いのだろう？S3？
- スタック内容更新はどうやっていくのだろう？
    - [AWS CloudFormation スタックの更新 - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks.html)
    - template ファイル上書きやエディターでの更新ができる
        - 差分詳細はわからないの？
- CFnを使うとネットワーク周りを考えてから中身を考える順序が自然になる
- EC2が停止してもそれはdriftではないらしい
- ドキュメントで「更新に伴う要件」と書いてあるのが更新時の挙動らしい
- ドキュメント項目多すぎて調べにくい
- 公式リンター・文法チェック [awslabs/cfn-python-lint: CloudFormation Linter](https://github.com/awslabs/cfn-python-lint)
- シェルスクリプトを実行するあたりで、例えば S3 のマウントとか作業があればやってしまっても良いかもしれない。

### 参考ブログ
- [【CloudFormation入門1】5分と6行で始めるAWS CloudFormationテンプレートによるインフラ構築 ｜ DevelopersIO](https://dev.classmethod.jp/cloud/aws/cloudformation-beginner01/)