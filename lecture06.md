loudTrail### 最後にAWSを利用した日の記録
* イベント履歴
![cloudtrail](/img/lecture06/ct/cloudtrail.png)
#### ConsoleLogin
* AWS Management Consoleにブラウザ経由でログインした時の記録
![ConsoleLogin](/img/lecture06/ct/CL.png)

#### CheckMfa
* 要素認証（MFA）をチェックまたは検証する際に記録
![CheckMfa](/img/lecture06/ct/CM.png)

#### ModifyTemporaryCredentialsOnEnvironmentEC2
* EC2 環境における一時的な認証情報の変更 
* ![MTCOEEC2](/img/lecture06/ct/MCTOE.png)



## CloudWatch### CloudWatchを使用してALBのアラームとOKアクションを設定
* AmazonSNS
![AmazonSNS](/images/lecture06/cw/sns.png)
* アクションの設定
![action](/images/lecture06/cw/cwaction.png)
#### Action: アラーム
* ターゲットグループのヘルスステータス: Unhealthy
![cw_unhealthy-tg](/images/lecture06/cw/tgunhealty.png)
* CloudWatchのアクション: アラーム状態
![cw_alarm-UnHealthyHostCount](/images/lecture06/cw/testaleart1.png)
![cw_alarm-UnHealthyHostCount](/images/lecture06/cw/testaleart2.png)
* メール通知
![cw_mail-alarm](/images/lecture06/cw/alearmmail.png)
#### Action: OK
* ターゲットグループのヘルスステータス：Healthy
![cw_healthy-tg](/images/lecture06/cw/tghealthy.png)
* CloudWatchのアクション：OK
![cw_ok-UnHealthyHostCount](/images/lecture06/cw/healthytest.png)
* メール通知
![cw_mail-ok](/images/lecture06/cw/okmail.png)

## AWSのコスト管理
* AWS利用料の見積<br>
[見積り](https://calculator.aws/#/estimate?id=3bf5941cdac78eed06741682c8d21a5279cdbc63)
* 現在の利用料金
![cost11](/images/lecture06/cost/cost11.png)
* 先月のEC2利用料金
![cost10](/images/lecture06/cost/cost10.png)
※ EC2の費用に関しては、一つのEC2は常に実行中、加えて複数のEC2を実行していた時期があり、無料枠の750時間を超えたため費用が発生した。



