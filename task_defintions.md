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


