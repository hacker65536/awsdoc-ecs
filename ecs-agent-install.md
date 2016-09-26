#Installing the Amazon ECS Container Agent
もしAmazon ECS container agentが含まれているAMIから起動してない場合は以下の手続きでinstallが出来る。

**Note** Amazon ECS container agentは Amazon ECS-optimized AMIに含まれているのでinstallの必要性はない。

**Amazon Linux ECS instanceに Amazon ECS containe agentをインストールする**
1. ECSにアクセス出来るIAM role割り当てられたAmazon linux instanceを立ち上げる。詳細[Amazon ECS Container Instance IAM Role](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance_IAM_role.html)を参照
2. instanceに接続する
3. __ecs-init__パッケージをインストールする。詳細は[view the source code onGitHub](https://github.com/aws/amazon-ecs-init)
```
sudo yum install -y ecs-init
```
4. Docker daemonを起動する
```
sudo service docker start
```
5. __ecsinit__ upstart jobの起動
```
sudo start ecs
```
6. agentの起動とinformationをagent introspection APIから確認出来る。(optional)詳細は[ Amazon ECS Container Agent Introspection](ecs-agent-introspection.md)を参照
```
curl http://localhost:51678/v1/metadata
{
  "Cluster": "default",
  "ContainerInstanceArn": "<container_instance_ARN>",
  "Version": "Amazon ECS Agent - v1.12.2 (ecda8a6)"
}
```

**Amazon Linux EC2 instance以外にinstallする場合**
1. Amazon ECS をアクセスできる IAM roleが割当られるEC2 instanceを起動する。詳細は[Amazon ECS Container Instance IAM Role](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance_IAM_role.html)を参照
2. instance に接続する
3. instance にDockerをインスントールする。Amazon ECS は Docker version 1.5.0以上が必要(推奨バージョンは1.11.2)。多くのsystemのpackage manager、例えばyumやapt-getはこのバージョンを持たない。dockerの最新版を特定のLinux ディストリビューションにインスントールする情報については[http://docs.docker.com/installation/](https://docs.docker.com/installation/)を参照。

**note**
