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
3. instance にDockerをインスントールする。Amazon ECS は Docker version 1.5.0以上が必要(推奨バージョンは1.11.2)。多くのsystemのpackage manager、例えばyumやapt-getはこのバージョンを持たない。dockerの最新版を特定のLinux ディストリビューションにインスントールする情報については[http://docs.docker.com/installation/](https://docs.docker.com/installation/)を参照。<br />**note**　Amazon Linux AMI はいつもECSのためのおすすめのDockerのバージョンが含まれている。**sudo yum install docker -y** を使用してAmazon LinuxにDcokerをインストールすることができる。
4. 必要最小バージョンの確認をする
```
$ sudo docker version
Client version: 1.4.1
Client API version: 1.16
Go version (client): go1.3.3
Git commit (client): 5bc2ff8
OS/Arch (client): linux/amd64
Server version: 1.4.1
Server API version: 1.16
Go version (server): go1.3.3
Git commit (server): 5bc2ff8
```
このサンプルでは、Dcokerのversionは1.4.1、これは最小バージョンの1.5.0より下になる。このinstanceはシステムのupgradeが必要となる。dockerの最新版を特定のLinux ディストリビューションにインスントールする情報については[http://docs.docker.com/installation/](https://docs.docker.com/installation/)を参照。<br />
5. 以下のコマンドをcontainer instanceで実行してtaskのためのIAM rolesを有効にする。詳細は[IAM Roles for Task](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)を参照
```
sysctl -w net.ipv4.conf.all.route_localnet=1
iptables -t nat -A PREROUTING -p tcp -d 169.254.170.2 --dport 80 -j DNAT --to-destination 127.0.0.1:51679
iptables -t nat -A OUTPUT -d 169.254.170.2 -p tcp -m tcp --dport 80 -j REDIRECT --to-ports 51679
```
6. Amazon ECS container agent