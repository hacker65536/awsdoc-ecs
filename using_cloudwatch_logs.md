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
インストールできれば次のセクションに進みagentの設定を行う
##Configuring and Starting the CloudWatch Logs Agent

CloudWatch Logs agentの設定ファイル(/etc/awslogs/awslogs.conf)がCloudWatch Logsにログファイルを送る設定を記述している。[general]セクションで全ログストリームに関する一般的な設定を行う。個別にモニターしたいログストリームあればセクションに追加する。詳細は[CloudWatch Logs Agent Reference](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/AgentReference.html)を参照。

下記はAmazon ECS-optimized AMI用の設定ファイルでいくつか一般的なログストリームを設定する

/var/log/dmseg  
>Linux kernelのbuffer message 
  
/var/log/messages

>Global system messages

/var/log/docker  

>Docker daemon log message

/var/log/ecs/ecs-init.log
>upstartジョブのecs-initのlog

/var/log/ecs/ecs-agent.log
>Amazon ECS container agentのlog

/var/log/ecs/audit.log
>task credential providerのIAM rolesからのログ

下記のサンプルファイルを使用する場合は__{cluster}__と__{continer_instance_id}__を置き換えること。各個のcontainer instanceはcluster名によってグルーピングされる。サンプルファイルの下にcluster名とcontainer instance IDのリプレースの手順がある。

```ini
[general]
state_file = /var/lib/awslogs/agent-state        
 
[/var/log/dmesg]
file = /var/log/dmesg
log_group_name = /var/log/dmesg
log_stream_name = {cluster}/{container_instance_id}

[/var/log/messages]
file = /var/log/messages
log_group_name = /var/log/messages
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %b %d %H:%M:%S

[/var/log/docker]
file = /var/log/docker
log_group_name = /var/log/docker
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%S.%f

[/var/log/ecs/ecs-init.log]
file = /var/log/ecs/ecs-init.log.*
log_group_name = /var/log/ecs/ecs-init.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

[/var/log/ecs/ecs-agent.log]
file = /var/log/ecs/ecs-agent.log.*
log_group_name = /var/log/ecs/ecs-agent.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

[/var/log/ecs/audit.log]
file = /var/log/ecs/audit.log.*
log_group_name = /var/log/ecs/audit.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ
```

**CloudWatch Logs agentの設定を行う**
1. 既存の設定ファイルのバックアップを行う<br />
```bash
sudo mv /etc/awslogs/awslogs.conf{,.bak}
```
2. 空のファイルを作る <br />
```bash
sudo touch /etc/awslogs/awslogs.conf
```
3. /etc/awslogs/awslogs.confをテキストエディタで開き上記のサンプルを挿入する
4. **jq**(json utility)をインストール<br />
```bash
sudo yum install -y jq
```
5. cluster名をAmazon ECS introspection APIに問い合わせし、環境変数にsetする<br />
```bash
cluster=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .Cluster')
```
6. 前のステップでsetした環境変数を{cluster}プレースホルダーとリプレースする<br />
```
sudo sed -i -e "s/{cluster}/$cluster/g" /etc/awslogs/awslogs.conf
```
7. container IDを Amazon ECS introspection APIに問合せして環境変数にsetする
```
container_instance_id=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .ContainerInstanceArn' | awk -F/ '{print $2}' )
```
8. {container_instance_id}プレースホルダーを前のステップでset下環境変数とリプレースする
```
sudo sed -i -e "s/{container_instance_id}/$container_instance_id/g" /etc/awslogs/awslogs.conf
```

**CloudWatch Logs agentのリージョンを設定する**
defaultではCloudWatch Logs agentは __us-east-1__にデータを送る様になっている。もしclusterがある別のリージョンに送りたいのであれば/etc/awslogs/awscli.confファイルにリージョンをsetする
1. /etc/awslogs/awscli.confをファイルエディターで開く
2. [default]セクションのus-east-1を適宜に置き換えてる
3. saveする

**CloudWatch Logs agentを起動する**
1. CloudWatch Logs agentを下記のコマンドでスタートさせる
```
sudo service awslogs start
```
2. chekconfigコマンドでリブート時にCloudWatch Logs agentが起動するように設定する
```
sudo chkconfig awslogs on
```

##Viewing CloudWatch Logs
CloudWatch Logs にログを送る権限を正しく設定したロールがcontainer instanceに割り当てられ、agentが起動するように設定していれば、container instanceからlog dataが CloudWatch Logsに送られるようになるだろう。Amazon Management Consoleでログの確認とサーチができるようになる。

**note** 新しいinstanceはログ送信するのに数分かかることがある

**CloudWatch Logs データの見方**
1. CloudWatch consoleを開く
2. 左のナビゲーションバーから**Logs**を選択
3. 設定したlog groupを確認できる
4. 確認したいlog groupを選択
5. みたい log steamを選択する。ストリームは クラスタ名と container instance IDによって識別される

##Configuring CloudWatch Logs at Launch with User Data
Amazon EC2で Amazon ECS container instanceをランチする場合、user dataをinstanceに渡すオプションを使えば、個々のinstanceの起動後に走るスクリプトによって自動的にtaskの設定が実行される。幾つかのtypeのuser dataをinstanceに渡すことが出来る。shellscriptを含めたり、cloud-init ディレクティブとupstart jobs。また ランチウィザードにplain textとして、ファイルとして(cli)、また base64-encoded text(API calls)渡すことが出来る。

サンプルuser dataは以下のタスクを実行する
* awslogs パッケージのinstall(CloudWatch Logs agentが同梱される)
* install jq Json query utility
* CloudWatch Logs agentの設定ファイルの書換え、データの送るべきリージョンの設定
* Cluster名とAmazon ECS cotainer agentが起動後のCotainer instance IDを取得し、CloudWatch Logs agentの設定ファイルのlog streamにそれらの値を書き込む
* CloudWatch Logs agentを起動
* system がbootする度に起動するよう設定する

```
Content-Type: multipart/mixed; boundary="==BOUNDARY=="
MIME-Version: 1.0

--==BOUNDARY==
MIME-Version: 1.0
Content-Type: text/text/x-shellscript; charset="us-ascii"
#!/bin/bash
# Install awslogs and the jq JSON parser
yum install -y awslogs jq

# Inject the CloudWatch Logs configuration file contents
cat > /etc/awslogs/awslogs.conf <<- EOF
[general]
state_file = /var/lib/awslogs/agent-state        
 
[/var/log/dmesg]
file = /var/log/dmesg
log_group_name = /var/log/dmesg
log_stream_name = {cluster}/{container_instance_id}

[/var/log/messages]
file = /var/log/messages
log_group_name = /var/log/messages
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %b %d %H:%M:%S

[/var/log/docker]
file = /var/log/docker
log_group_name = /var/log/docker
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%S.%f

[/var/log/ecs/ecs-init.log]
file = /var/log/ecs/ecs-init.log.*
log_group_name = /var/log/ecs/ecs-init.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

[/var/log/ecs/ecs-agent.log]
file = /var/log/ecs/ecs-agent.log.*
log_group_name = /var/log/ecs/ecs-agent.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

[/var/log/ecs/audit.log]
file = /var/log/ecs/audit.log.*
log_group_name = /var/log/ecs/audit.log
log_stream_name = {cluster}/{container_instance_id}
datetime_format = %Y-%m-%dT%H:%M:%SZ

EOF

--==BOUNDARY==
MIME-Version: 1.0
Content-Type: text/text/x-shellscript; charset="us-ascii"
#!/bin/bash
# Set the region to send CloudWatch Logs data to (the region where the container instance is located)
region=$(curl 169.254.169.254/latest/meta-data/placement/availability-zone | sed s'/.$//')
sed -i -e "s/region = us-east-1/region = $region/g" /etc/awslogs/awscli.conf

--==BOUNDARY==
MIME-Version: 1.0
Content-Type: text/text/upstart-job; charset="us-ascii"

#upstart-job
description "Configure and start CloudWatch Logs agent on Amazon ECS container instance"
author "Amazon Web Services"
start on started ecs

script
	exec 2>>/var/log/ecs/cloudwatch-logs-start.log
	set -x
	
	until curl -s http://localhost:51678/v1/metadata
	do
		sleep 1	
	done

	# Grab the cluster and container instance ARN from instance metadata
	cluster=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .Cluster')
	container_instance_id=$(curl -s http://localhost:51678/v1/metadata | jq -r '. | .ContainerInstanceArn' | awk -F/ '{print $2}' )
	
	# Replace the cluster name and container instance ID placeholders with the actual values
	sed -i -e "s/{cluster}/$cluster/g" /etc/awslogs/awslogs.conf
	sed -i -e "s/{container_instance_id}/$container_instance_id/g" /etc/awslogs/awslogs.conf
	
	service awslogs start
	chkconfig awslogs on
end script
--==BOUNDARY==--
```

もし__ECS-CloudWatchLogs__ポリシーを作成しているのなら、[CloudWatch Logs IAM Policy](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_cloudwatch_logs.html#cwl_iam_policy)で説明した__ecsInstanceRole__にアタッチすると、上記のuser dat blockを手動で起動するどのcontainer instanceにも追加できる。また Auto Scaling launch configurationにも追加できる。このuser dataで起動した cotainer instanceは起動後しばらくすると log data をCloudWatch Logs に送るようになる。詳細は[Launching an Amazon ECS container instance](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/launch_container_instance.html)


