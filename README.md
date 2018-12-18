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
- Chapter.3 サーバーを構築する
- Chapter.4 Web サーバーソフトをインストールする
- Chapter.6 プライベートサブネットを構築する
- Chapter.7 NATを構築する
- Chapter.8 DB を用いたブログシステムの構築