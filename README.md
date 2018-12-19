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
- Chapter.4 Web サーバーソフトをインストールする
- Chapter.6 プライベートサブネットを構築する
- Chapter.7 NATを構築する
- Chapter.8 DB を用いたブログシステムの構築