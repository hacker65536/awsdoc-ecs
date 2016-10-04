#Scaling a Cluster
もし、12/24/2015以降の[Getting Started with Amazon ECS]()に説明された体験を最初に実行したならば、container instanceの増減によるスケールアップとダウンがCloudFormation stackで作られ、配置される。Amazon ECS コンソールからこれらのスケーリングオペレーションを実行出来る。

もし12/24/2015以降の[Getting Started with Amazon ECS]()に説明された手順で実行されていなければ、Amazon ECS コンソールからスケールすることはできない。しかしながら、Auto Scaling コンソールで既存のAuto Scaling groupを変更して関連付させる事ができる。手動で container instanceをAmazon EC2 コンソールから起動や削除を行うことが出来る。詳細は[Laungching an Amazon ECS Container Instance](launch_container_instance.md)

**クラスタをスケールする**
1. ECS コンソールを開く
2. ナビゲーションバーからクラスタがあるregionを選択
3. ナビゲーションパネルから**Clusters**を選択
4. スケールしたClusterを選択
5. **Cluster:name**ページでECS instanceタブを選択<br /><br />
もし**Scale ECS instance**ボタンが表示されていれば、次のステップでクラスタをスケールすることが出来る。もしなければ手動でAutoScaling groupを調整しインスタンスのスケーリング行うか、EC2 コンソールで手動でcontainer instanceの起動とターミネートを行う。
6. **Scale ECS instances**を選択
7. **Desired number of instances**フィールドでスケールしたいクラスタのinstanceの数を入力し、**Scale**を選択<br /><br />
**Note**  
もしcluster内のcontainer instanceの数を減らしたいのであれば、終了したinstance上で実行中のすべてのtaskは停止する。