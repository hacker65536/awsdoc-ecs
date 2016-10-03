#Creating a Task Definition
Amazon ECS上でDocker containersを起動する前にtask definitionを作る必要がある。
**新しい task definition を作る**
1. Amazon ECS コンソールを開く
2. ナビゲーショバーから task definitionを登録したいregionを選択する
3. ナビゲーションパネルから**Task Definitions**を選択
4. **Task Definitions**ページで**Create new Task Definition**を選択
5. (オプション)もしJSON形式のtask defnitionを使いたい場合以下の手順
  6. **Create a Task Defnition**ページでページの下までスクロールして**Configure via JSON**を選択
  7. JSON形式のtask definitionをテキストエリアにペーストして**Save**する
  8. 情報を確認して**Create**
6. **Task Defnition Name**フィールドで task definitionの名前を入力、255文字(大文字、小文字)、数字、ハイフン、アンダースコアが可能
7. (オプション)**Task Role**フィールドでは、taskで作られたコンテナがAWS APIsを呼び出すための権限を提供するIAM roleを選択。詳細は[IAM Roles for Tasks](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html)を参照<br /><br />
**note** ここでは**Amazon EC2 Container Service Task Role**をもったroleが示されている。TaskのためのIAM Roleの作り方は[Creating an IAM Role and Policy for your Tasks](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/task-iam-roles.html#create_task_iam_policy_and_role)を参照
8. (オプション) **Network Mode**フィールドでは、使いたいDocker network modeを選択。対応可能な network mode はDocker run referenceの[Network settings](https://docs.docker.com/engine/reference/run/#/network-settings)で説明している<br /><br />
defaultのDcoker network modeは__bridge__となっている。もし network mode が __none__がセットされていると catiner definitonではport mappingsを指定できない。そしてtaskのコンテナは外部への接続を持たない。
