# RaiseTech　AWSフルコース学習内容
2023年9月9日より、オンラインスクール「[RaiseTech](https://raise-tech.net/courses-lp/aws-full-course)」にてAWSコースの受講を開始。<br>
第16回まである授業の内、第14回まで受講済み。
最終課題である第13回は現在取り組み中。<br>

<br>
<br>

| 授業№ | 主な学習内容 | アウトプット |
|:---:|---|---|
| 1 | - AWSとは | - AWSアカウント作成<br> - IAMの推奨設定<br> - Cloud9の作成<br> - Rubyを実行して「Hello World」を表示 |
| 2 | - バージョン管理システム<br> - Git<br> - GitHub<br> - Markdown | **[lecture02](./lecture02.md)**<br> - GitHubアカウントを作成<br> - Git設定<br> - Pull Request |
| 3 | - Webアプリケーションとは<br> - システム開発の流れ<br> - 外部ライブラリと構成管理の重要性 | **[lecture03](./lecture03.md)**<br> - Cloud9を使用してのRailsサンプルアプリケーションのデプロイ<br> - APサーバー、DBサーバーについて |
| 4 | - AWSでの権限管理<br> - EC2・RDSとは | **[lecture04](./lecture04.md)**<br> - IAM権限管理とAWS上でのネットワーク ～ EC2、RDSの作成 |
| 5 | - ELB・S3について<br> - インフラ構成図 | **[lecture05](./lecture05.md)**<br> - EC2上でアプリケーションのデプロイ<br> - ELBとS3の構築<br> - 構成図の作成 |
| 6 | - システムの安定稼働とAWSでの実装・確認 | **[lecture06](./lecture06.md)**<br> - CloudTrailのイベント確認<br> - CloudWatchアラームの動作確認<br> - AWS利用料の見積書を作成<br> - コスト管理|
| 7 | - システムにおけるセキュリティの基礎<br> - セキュリティ対策 | **[lecture07](./lecture07.md)**<br> - 考察と対策|
| 8 | - 第4回・第5回授業内容の実演 | ― |
| 9 | - 第4回・第5回授業内容の実演 | ― |
| 10 | - CloudFormation | **[lecture10](./lecture10.md)**<br> - AWS環境のコード化 |
| 11 | - インフラのコード化を支援するツール<br> - インフラのテストとは<br> - テスト駆動環境<br> - ServerSpec | <br>- ServerSpec のテスト |
| 12 | - Terraformの解説<br> - DevOps<br> - CI/CDツールとは | <br>- CircleCI のサンプルコンフィグの組み込み |
| 13 | - 構成管理ツールとは<br> - Ansible<br> - OpsWorks<br> - CircleCIとの併用 | **学習中**<br>- CircleCI のサンプルに ServerSpec や Ansible の処理を追加 |
| 14 | - 第13回授業内容の実演 | <br> - AWS 構成図、自動化処理がわかる図、リポジトリのREADME作成 |
| 15 | - 第13回授業内容の実演 | **学習予定** |
| 16 | - 現場へ出ていくにあたって | **学習予定** |

<br>
<br>
<br>

## CloudFormationで構築した環境

<br>
<br>

![lecture05_diagram](/img/lecture05/diagram/lecture05.drawio.png)

<br>
<br>
<br>

## 直近の完了課題
### [第10回](./lecture12.md)課題
第5回の課題で構築した環境（上図）をCloudFormationでコード化し、自動で環境が構築されることを確認。<br>
<br>
### [第11回](./lecture11.md)課題
ServerSpecを使用して第５回で構築した環境をテスト。
<br>
### [第12回](./lecture12.md)課題
CircleCIを使用し第10回の課題で作成した`yml`ファイルをチェック。
<br>


<br>

### 【 工夫した点 】
- [第10回](./lecture10.md)の課題、Cloudformationのテンプレートに関しては、極力パラメーターの入力だけでスタック作成をできるようにした。
- [第5回](./lecture05.md)、[第11回](./lecture11.md)、[第12回](./lecture12.md)の課題は、誰でも再現できるよう手順を細かく記載した。
- リポジトリをわかりやすく極力シンプルにまとめた。

<br>

<br>

## 現在取り組んでいる課題の構成図
![lecture05_diagram](/img/lecture05/diagram/lecture05.drawio.png)

<br>
<br>

## 2024年9月現在
- AWS Certified Solutions Architect - Associate 取得済み
- RaiseTech AWSコース 第13回課題取り組み中
- RaiseTech受講生同士のチーム開発に参加中# raisetech
My First repository
