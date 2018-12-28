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
    - 起動中のスタックにDB用のEC2をプライベートサブネットに設置する。
    - MyPrivateSubnet のルートテーブルはデフォルトのままでOK。
    - セキュリティグループ「任意の場所」を設定したいときに「0.0.0.0/0, ::/0」はダメ。
        - 0.0.0.0/0 で同じ意味という事か？
    - WEBサーバーからDBサーバーへの疎通確認はping
    - pem アップロードして踏み台アクセス
    - [ ] RDSをプライベートサブネットに設置してみる
    - [ ] ssh-agent 経由で多段アクセスしてみる

- Chapter.7 NATを構築する
    - PrivateSubnet から PublicSubnet に配置してある NATgateway を通じてインターネットにアクセスする。InternetGatewayと同レイヤー？
    - Ch6状態でネット未接続。DBサーバーはパッケージインストールできない？
        - yum info 無理 OK
        - lsof -i -n -P 確認 OK
    - 新規起動したらEC2のAZがバラつく
        - [x] AZ指定
            - Subnet と EC2 に AvailabilityZone: ap-northeast-1d
    - [AWS::EC2::NatGateway - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-ec2-natgateway.html)
        - 専用 EIP と所属 SubnetId を指定
    - [Fn::GetAtt - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference-getatt.html)
        - 対象リソースから指定した属性値を取得する
        - 短縮 `!GetAtt logicalNameOfResource.attributeName`
    - [x] 疎通確認
        - yum info , curl

- Chapter.8 DB を用いたブログシステムの構築
    - 7章までのDB用インスタンスではなく、RDSを配置して利用する
    - 何か経由で固定IPをRDSに通す
    - [AWS::RDS::DBInstance - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-rds-database-instance.html)
        - EC2Instance 並に項目が多いリソース。
        - Required / Conditional
            - AllocatedStorage
            - DBInstanceClass
            - Engine
            - Iops (io1 only)
            - MasterUsername
            - MasterUserPassword
            - MonitoringInterval
            - MonitoringRoleArn
            - StorageEncrypted
        - DeletionPolicy
            - スタック削除時の動作。本番環境ではSnapshotで再利用がベター？
            - 削除時に毎回バックアップつくるから重いので開発時は外すのが良さそう
        - Tag Name ではなく DBName を使う
        - VPCSecurityGroups と DBSecurityGroups
        - [Amazon RDS セキュリティグループによるアクセスの制御 - Amazon Relational Database Service](https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html)
            - プラクティスが古い（DBSecurityGroups利用）
            - VPCSecurityGroupsを利用しておけばとりあえずOK
        - timezoneとかもここで指定できる
        - dbの表示名は DBInstanceIdentifier
    - EC2 サブネットと RDS サブネットは別？
        - EC2::Subnet を利用する点は同じ
        - 紐付ける RDS::DBSubnetGroup がRDS専用
    - VPCを指定したいとき直接VPCが指定できない。SubnetGroupからつくる。
        - Subnetを2つつくる(VPC指定, AZ複数必須)
            - どのくらいアドレス割り当てるといいのかね？
            - EC2::SubnetなのでEC2と共有しても良い。
        - SubnetGroupをつくる（↑2つを紐付け）
            - [AWS::RDS::DBSubnetGroup - AWS CloudFormation](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-resource-rds-dbsubnet-group.html)
        - RDS::DBInstance の DBSubnetGroupNameに指定する
    - [x] 疎通確認  WebEC2 => MyRDS mysqlログイン OK
    - [x] VPC外からアクセス不能 OK

### MEMO
- yamlファイルどこに置いておくのが良いのだろう？S3？
- 複数テンプレートに分けてもTemplateURLで参照できるらしい？
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
- リソース属性値のデバッグどうすればええんや？
- EIP を維持したまま Replace 更新すると失敗する。付け替え不能。
- EIP の Tag Name 設定したいが。
- CIDR 名前の由来
- ${EnvName} 環境変数的な物がとれるらしい?
    - Parameters: で宣言し、!Ref で呼び出し。
    - !Sub "${AWS::StackName}-ops-iam-role" とすると変数埋め込み
    - 疑似パラメーター例
        - AWS::AccountId アカウントID
        - AWS::Region リージョン文字列
        - AWS::StackName スタック名称
        - AWS::StackId スタックID
        - AWS::NoValue 未定義値
- LAMPベストプラクティスで LaunchConfiguration なるリソースを利用して bootstrap してる
- バリデーション以上につくらないと動作わからない。
    - しかし何度もつくって試すのめんどい => CLIでやるのが良いとか
    - 公式のコレが使えるらしい [aws-quickstart/taskcat: Test all the CloudFormation things! (with TaskCat)](https://github.com/aws-quickstart/taskcat)
- EC2::Subnet 存在すると作成できないで停止する
    - 使い回すとか気を利かせる機能ではなく、つくるかつかうか。
    - 名前衝突する
    - 名前衝突はテンプレ変数で回避できそう？
- スタック更新は ChangeSet という仮実行手順を利用する。
- リソース論理IDとTagNameは違うので、論理IDはtemplate内部の役割で命名する
- 複製可能にするためにはどうすればいいの？
    - 論理IDは重複できる
    - リソース名の属性パラメーターは重複できない
    - VPCのCIDR範囲は重複しないように作る
        - CIDR アドレスの一部だけParameters入力できる様にすれば？
- DBNameは英数のみなので、StackName利用に注意
- EC2 InstanceInitiatedShutdownBehavior
    - シャットダウン時の動作モード Default: stop
- EC2 削除保護の有効化 DisableApiTermination
- S3 のストレージクラス指定どうやるん
- ACM 無料証明書はCNAME設定するまで待機してくれる


### 参考ブログ
- [【CloudFormation入門1】5分と6行で始めるAWS CloudFormationテンプレートによるインフラ構築 ｜ DevelopersIO](https://dev.classmethod.jp/cloud/aws/cloudformation-beginner01/)
- [AWS CloudFormation VPC テンプレート - AWS CodeBuild](https://docs.aws.amazon.com/ja_jp/codebuild/latest/userguide/cloudformation-vpc-template.html)
- [ある程度の規模で運用するAWS CloudFormationの勘所 - Qiita](https://qiita.com/datake914/items/14cb4d66570c1efcbc7d#cloudformation%E3%81%AE%E9%81%A9%E7%94%A8%E7%AF%84%E5%9B%B2)
- [CloudFormationで削除保護をつけたEC2の作成と削除 ｜ DevelopersIO](https://dev.classmethod.jp/cloud/aws/creating_and_deleting_ec2_with_delete_protection_in_cloudformation/)