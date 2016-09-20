#Using CloudWatch Logs with Container Instances
ログ情報をCloudWatch Logsに送るようにcontainer instanceに設定することが出来る。これは複数のcontainer instanceの異なるログを一箇所で見ることを可能にする。このトピックはAmazon ECS-optimized AMIで起動したcontainer instanceで CloudWatch Logsを使用する方法を説明する。

taskからのcontainer logをCloudWatch Logsに送るには[Using the awslogs Log Driver](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_awslogs.html?shortFooter=true)を参照。詳細は[Monitoring Log Files](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)を参照。

Topics
 * [CloudWatch Logs IAM Policy]
 * [Installing the CloudWatch Logs Agent]
 * [Configuring and Starting the CloudWatch Logs Agent]
 * [Viewing CloudWatch Logs]
 * Configuring CloudWatch Logs at Launch with User Data]

##CloudWatch Logs IAM policy
container instanceがCloudWatch Logsにログを送るにはまずはcontainer instanceがCloudWatch Logs APIを使用できるようにIAM policyを作り、ecsInstanceRoleにアタッチし割りあてる必要がある。

**ECS-CloudWatchLogsのIAM policyの作成**
1. IAMコンソールを開く
2. ナビゲーションパネルからpoliciesを選択
3. **Create Policy**を選択
4. **Create your Own Policy**を選択
5. **Review Policy**ページで下記の情報を入力し**Create policy**を選択
  6. **Policy Name**フィールドに__ECS-CloudWatchLogs__
  7. **Policy Document**フィールドに下記のpolicyをペースト<br />
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogStreams"
            ],
            "Resource": [
                "arn:aws:logs:*:*:*"
            ]
        }
    ]
}
```

__ECS-CloudWatchLogs__ policyを __ecsInstanceRole__にアタッチする
1. IAMコンソールを開く
2. ナビゲーションパネルから**Roles**を選択
3. __ecsInstanceRole__を選択、もしなければ[Amazon ECS Container Instance IAM Role](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance_IAM_role.html)に従ってroleを作成する
4. **Permissions**タブを選択
5. **Managed Policies**セクションで**Attach Policy**を選択
6. **Filter**ボックスに**ECS-CloudWatchLogs**を入力しアタッチできるポリシーを絞る
7. **ECS-CloudWatchLogs**ポリシーの左のチェックボックスをチェックし**Attach policy**を選択

##Installing the CloudWatch Logs Agent
__ECS-CloudWatchLogs__policyをecsInstanceRoleに追加したら container instanceにCloudWatch Logs agentのインストールを行う
**note** この手順はAmazon ECS-optimized AMIのためのもので、別のOSに入れる場合は[Getting Started with CloudWatch Logs](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/CWL_GettingStarted.html)を参照

**CloudWatch Logs agentをインストール**

* 下記のコマンド実行しインストールを行う<br />
```
sudo yum install -y awslog
```