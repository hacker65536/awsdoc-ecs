#Amazon ECS Container Agent
Amazon ECS cantainer agentはcontainer instanceがclusterに接続できる事を許可する。Amazone ECS container agent は Amazon ECS-optimized AMIに含まれている。しかし、Amazon ECS がサポートで指定しているEC2 instanceにinstallすることも可能。Amazon ECS container agent がサポートしているのは EC2 instanceのみ。

**note** Amazone ECS containe agentのソースコードは[GitHub](https://github.com/aws/amazon-ecs-agent)に公開している。私達はincludeしたいプルリクを推奨している。しかしAWSは改変したコピーの仕様についてはサポートしていない。

**Topics**
* [Amazon ECS Container Agentのinstall](ecs-agent-install.md)
* [Amazon ECS Container Agentのバージョン](container_agent_versions.md)
* [Amazon ECS Container Agentのupdate](ecs-agent-update)
* [Amazon ECS Container Agentの設定](ecs-agent-config.md)
* [プラベートレジストリーの認証](private-auth.md)
* [Amazone ECS Container Agent の内観](ecs-agent-introspection.md)
* [HTTP Proxy Configuration](http_proxy_config.md)