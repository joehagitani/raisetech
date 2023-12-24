# 第10回課題

## CloudFormation を利用し、現在までに作った環境をコード化する。
* テンプレートをNetwork、Security、Application に分けて作成。

| CFnテンプレート|リソース内訳|
|:--|:--|
|[CFn_Network.yml](./tpl/lecture10_CFn/CFn_Network.yml)|⚫︎ VPC<br>⚫︎ InternetGateway<br>⚫︎ InternetGatewayAttachment<br>⚫︎ PublicSubnetA,C<br>⚫︎ PrivateSubnetA,C<br>⚫︎ PublicRouteTable<br>⚫︎ PrivateRouteTable(A,C)<br>⚫︎ PublicRoute<br>⚫︎ PublicSubnet(A,C)RouteTableAssociation<br>⚫︎ PrivateSubnet(A,C)RouteTableAssociation|
|[CFn-Security.yml](./tpl/lecture10_CFn/CFn-Security.yml)|⚫︎ EC2SG (EC2 Security Group)<br>⚫︎ ALBSG (ALB Security Group)<br>⚫︎ RDS Security Group (RDSSG)<br>⚫︎ ManagedPolicy (IAM Policy)<br>⚫︎Role (IAM Role)<br>⚫︎ InstanceProfile|
|[CFn-Application.yml](./tpl/lecture10_CFn/CFn-Application.yml)|⚫︎ NewKeyPair<br>⚫︎ Ec2Instance<br>⚫︎ ALB<br>⚫︎ ListenerHTTP<br>⚫︎ TargetGroup<br>⚫︎ KmsKey<br>⚫︎ RDSSecret<br>⚫︎ RDSSubnetGroup<br>⚫︎ RDSInstance<br>⚫︎ S3Bucket|
<br>

## 各リソースの内容説明

<br>
<br>

### [Network](./tpl/lecture10_CFn/CFn_Network.yml)
***
#### VPC
* 指定されたCIDRブロック (VPCCIDR) でVPCを作成。
* DNSサポートとホスト名の設定が有効。 
***
#### InternetGateway
* VPCにインターネットアクセスを提供するためのインターネットゲートウェイを作成。
***
#### InternetGatewayAttachment
* 作成したインターネットゲートウェイをVPCにアタッチ。  
***
#### PublicSubnetA,C 
* 指定されたCIDRブロック (PublicSubnetACIDR,PublicSubnetCCIDR) でパブリックサブネットを作成し、指定されたアベイラビリティゾーン (ap-northeast-1a,ap-northeast-1c) に配置。  
***
####  PrivateSubnetA,C
* 指定されたCIDRブロック (PrivateSubnetACIDR,PrivateSubnetCCIDR) でプライベートサブネットを作成し、指定されたアベイラビリティゾーン (ap-northeast-1a,ap-northeast-1c)に配置。  
***
#### PublicRouteTable
* VPC用のパブリックルートテーブルを作成。  
***
#### PrivateRouteTable(A,C)
* VPC用のプライベートルートテーブルを作成。  
***
#### PublicRoute
* パブリックルートテーブルにインターネットへのルート (0.0.0.0/0) を設定し、インターネットゲートウェイにルーティング。  
***
#### PublicSubnet(A,C)RouteTableAssociation
* パブリックサブネットをパブリックルートテーブルに関連付け。  
***
#### PrivateSubnet(A,C)RouteTableAssociation
* プライベートサブネットCをプライベートルートテーブルCに関連付け。  
***

<br>
<br>

### [Security](./tpl/lecture10_CFn/CFn-Security.yml)
***
#### EC2SG (EC2 Security Group)
* EC2インスタンス用のセキュリティグループ。
* ポート80（HTTP）、ポート22（SSH）、ポート3000に対してアクセスを許可。
* SSHとポート3000は特定のIP (MyLocalIP) からのみアクセス可能。  
***
#### ALBSG (ALB Security Group)
* アプリケーションロードバランサー(ALB)用のセキュリティグループ。
* ポート80（HTTP）に対して全てのIPからのアクセスを許可。  
***
#### RDSSG (RDS Security Group)
* RDS用のセキュリティグループ。  
* ポート3306（MySQLデータベース）に対して全てのIPからのアクセスを許可。  
***
####  ManagedPolicy (IAM Policy)
* S3バケット cfn-s3 に対する特定のアクセス権限を設定するIAMポリシー。
* バケットリスト表示、オブジェクトの追加/取得/削除が可能。  
***
#### Role (IAM Role)
* EC2インスタンスに割り当てるためのIAMロール。
* AmazonS3FullAccess ポリシーを含む。
* EC2がこのロールを引き受けることを許可。  
***
#### IInstanceProfile
* 上記のIAMロールをEC2インスタンスに割り当てるためのインスタンスプロファイル。    
***  

<br>
<br>

### [Application](./tpl/lecture10_CFn/CFn-Application.yml)
***
#### NewKeyPair
* EC2インスタンスのSSHキーを作成。  
***
#### Ec2Instance
* AMI IDを使用したEC2インスタンスを設定。
* t2.microタイプ、セキュリティグループ、パブリックIPの割り当て設定含む。  
***
#### ALB
* インターネット向けALBを作成。
* ポート80で受信し、定義されたセキュリティグループとサブネットを使用。  
***
#### ListenerHTTP
* ALBにHTTPリスナーを設定。
* ポート80で受信し、トラフィックをターゲットグループに転送。  
***
#### TargetGroup
* EC2インスタンスをターゲットとするターゲットグループを作成。
* HTTPプロトコルと健康チェック設定。  
***
#### KmsKey
* RDSインスタンスの暗号化に使用するKMSキーを作成。  
***
#### RDSSecret
* RDSデータベースのパスワードを管理するSecrets Managerシークレットを作成。  
***
#### RDSSubnetGroup
* RDSインスタンスのためのサブネットグループを作成。  
***
#### RDSInstance
* MySQLをエンジンとするRDSデータベースインスタンスを作成。
* ストレージ、セキュリティグループ、KMSキーの設定含む。  
***
#### S3Bucket
* S3バケットを作成。
* サーバーサイド暗号化とアクセスコントロール設定含む。  
***  

<br>
<br>

## 各スタックの構築確認
### [Network](./tpl/lecture10_CFn/CFn_Network.yml)
* Networkスタック作成
![network_stack](./img/lecture10/network/network_stack.png)  

  * VPC
![VPC](./img/lecture10/network/VPC.png)  

<br>
<br>

### [Security](./tpl/lecture10_CFn/CFn-Security.yml)
* Securityスタック作成
![Security_stack](./img/lecture10/security/security_stack.png)  

  * EC2 Security Group
![EC2SG](./img/lecture10/security/EC2SG.png)  

  * ALB Security Group
![ALBSG](./img/lecture10/security/ALBSG.png)  

  * RDS Security Group
![RDSSG](./img/lecture10/security/RDSSG.png)  

  * IAM Policy
![IAMS3](./img/lecture10/security/IAMS3.png)  

  * IAM Role
![Role](./img/lecture10/security/Role.png)  

<br>
<br>


### [Application](./tpl/lecture10_CFn/CFn-Application.yml)   
* Applicationスタック作成
![Application_stack](./img/lecture10/app/app_stack.png)  

  * EC2
![EC2](./img/lecture10/app/EC2.png)  

  * ALB
![ALB](./img/lecture10/app/ALB.png)  

  * TG
![TG](./img/lecture10/app/TG.png)  

  * RDS
![RDS](./img/lecture10/app/RDS.png)  

  * Secrets Manager
![ASM](./img/lecture10/app/ASM.png)  

  * KMS
![KMS](./img/lecture10/app/KMS.png)  

  * S3
![S3](./img/lecture10/app/S3.png)  

