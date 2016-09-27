#Amazon ECS Task Definitions
task definition は Amazon ECSで Docekr containerを実行するために必要である。いくつかのパラメータをtask definitionで指定できる。

* taskがどのDocker imageを使うか
* 個々のコンテナがどれくらいCPUとメモリを仕様するか
* コンテナで使用するDocker networking　モード
* コンテナからの何のポートがホスト container instanceへのマップされているか
* コンテナが終了、または失敗した時にtaskを続けるかどうか
* コンテナが起動時に実行すべきコマンド
* コンテナが起動時に渡されるべき環境変数
* taskのコンテナでどのデータボリュームを使用すべきか
* taskがどのIAM roleを使うべき

task definitionで複数のコンテナとデータボリュームを指定することが出来る。task definitionの詳細のパラメータは[Task Definition Parameters](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html)を参照

アプリケーション全体が一つのtask definitionに存在する必要はないが多くはそうではない。複数の関連した個々の単一のコンポーネントのtask definitionを結合し、それをまたがったアプリケーションを構成できる。詳細は[Application Architecture](
