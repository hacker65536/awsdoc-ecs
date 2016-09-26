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

**Amazon Linux EC2 instance以外にinstallする場合**
1. 
