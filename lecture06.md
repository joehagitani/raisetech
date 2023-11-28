# 第6回課題 
## CloudTrail 
### 最後にAWSを利用した日の記録 
* イベント履歴
![cloudtrail](/img/lecture06/ct/cloudtrail.png)
### ConsoleLogin
* AWS Management Consoleにブラウザ経由でログインした時の記録
![ConsoleLogin](/img/lecture06/ct/CL.png)
### ConsoleLoginに含まれている内容３つをピックアップ
* イベントソース: signin.amazonaws.com
* 発信元IPアドレス: 118.110.141.197
* イベントID: 2025a10c-fbfd-41d1-a94a-e49650a02bba



## CloudWatch 
### CloudWatchを使用してALBのアラームとOKアクションを設定
* AmazonSNS
![AmazonSNS](/img/lecture06/cw/sns.png)
* アクションの設定
![action](/img/lecture06/cw/cwaction.png)
### Action: アラーム
* ターゲットグループのヘルスステータス: Unhealthy
![cw_unhealthy-tg](/img/lecture06/cw/tgunhealty.png)
* CloudWatchのアクション: アラーム状態
![cw_alarm-UnHealthyHostCount](/img/lecture06/cw/testaleart1.png)
![cw_alarm-UnHealthyHostCount](/img/lecture06/cw/testaleart2.png)
* メール通知
![cw_mail-alarm](/img/lecture06/cw/alearmmail.png)
### Action: OK
* ターゲットグループのヘルスステータス：Healthy
![cw_healthy-tg](/img/lecture06/cw/tghealthy.png)
* CloudWatchのアクション：OK
![cw_ok-UnHealthyHostCount](/img/lecture06/cw/healthytest.png)
* メール通知
![cw_mail-ok](/img/lecture06/cw/okmail.png)

## AWSのコスト管理
* AWS利用料の見積<br>
[見積り](https://calculator.aws/#/estimate?id=35e756fbef8fad1936e48916c050c43c05fc340e) 

※EC2、RDSを1日4時間の起動で見積り 

* 現在の利用料金
![cost11](/img/lecture06/cost/cost11.png)
* 先月のEC2利用料金
![cost10](/img/lecture06/cost/cost10.png)
※ EC2の費用に関しては、一つのEC2は常に実行中、加えて複数のEC2を実行していた時期があり、無料枠の750時間を超えたため費用が発生した。



