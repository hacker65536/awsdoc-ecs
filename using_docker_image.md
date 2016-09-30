#Using a Docker Image in a Task Definition
この項では[Docker Basics]()の記事で作った**amazon-ecs-sample**imageをもってどのようにtask definition を設定するか、デモンストレーションを行う。

**__amazonecs-sample__ imageを使ったtask definitionを登録する**
1. ecs-demo-php-simple-appディレクトリにあるsample-app-task-def.jsonを確認する
```json
{
    "family": "console-sample-app",
    "volumes": [
        {
            "name": "my-vol",
            "host": {}
        }
    ],
    "containerDefinitions": [
        {
            "environment": [],
            "name": "simple-app",
            "image": "amazon/amazon-ecs-sample",
            "cpu": 10,
            "memory": 500,
            "portMappings": [
                {
                    "containerPort": 80,
                    "hostPort": 80
                }
            ],
            "mountPoints": [
                {
                    "sourceVolume": "my-vol",
                    "containerPath": "/var/www/my-vol"
                }
            ],
            "entryPoint": [
                "/usr/sbin/apache2",
                "-D",
                "FOREGROUND"
            ],
            "essential": true
        },
        {
            "name": "busybox",
            "image": "busybox",
            "cpu": 10,
            "memory": 500,
            "volumesFrom": [
            {
              "sourceContainer": "simple-app"
            }
            ],
            "entryPoint": [
                "sh",
                "-c"
            ],
            "command": [
                "/bin/sh -c \"while true; do /bin/date > /var/www/my-vol/date; sleep 1; done\""
            ],
            "essential": false
        }
    ]
}
```
このtask defnition JSONファイルは２つのコンテナを記している。一つは**amazon-ecs-sample** imageを使っている。defaultではこのimageはAmazon Docker Hub repositoryからpullされる、しかし上記に指定されているamazon 自身のリポジトリに変更できる。
2. simple-app-task-def.jsonをtask defnitionとして登録する
```
aws ecs register-task-definition --cli-input-json file://simple-app-task-def.json
```