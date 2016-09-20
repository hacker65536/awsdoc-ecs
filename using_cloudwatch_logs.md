#Using CloudWatch Logs with Container Instances
ログ情報をCloudWatch Logsに送るようにcontainer instanceに設定することが出来る。これは複数のcontainer instanceの異なるログを一箇所で見ることを可能にする。このトピックはAmazon ECS-optimized AMIで起動したcontainer instanceで CloudWatch Logsを使用する方法を説明する。

taskからのcontainer logをCloudWatch Logsに送るには[Using the awslogs Log Driver](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_awslogs.html?shortFooter=true)を参照。詳細は[Monitoring Log Files](http://docs.aws.amazon.com/AmazonCloudWatch/latest/DeveloperGuide/WhatIsCloudWatchLogs.html)を参照。

Topics
 * [CloudWatch Logs IAM Policy
 * [Installing the CloudWatch Logs Agent
 * [Configuring and Starting the CloudWatch Logs Agent
 * [Viewing CloudWatch Logs
 * Configuring CloudWatch Logs at Launch with User Data