#Getting Started with Amazon ECS

Amazon ECS コンソールで、task definitionを作り、taskをスケジューリングし、クラスタを設定しAmazon EC2 Container Serviceを始めよう。

オプションでAmazon EC2 Container Registry (Amazon ECR)イメージレジストリを作り、イメージをpushできる。詳細は[Amazon EC2 Container Registry User Guide](http://docs.aws.amazon.com/AmazonECR/latest/userguide/)を確認。

Amazon ECS first run wizardはAmazon ECSを始めるため、手順を通じてガイドしてくれる。wizardはsample web applicationを起動するクラスタを作成してくれる。もしすでにAmazon ECSで起動したいDocker imageを持っているなら、そのイメージを使うtask definitionを作り自身のクラスタと切り替えられる。

**Important**

始める前に、[Setting Up with Amazon ECS](get-set-up-for-amazon-ecs.md)のステップを終わらせ、[Amazon ECS First Run Wizard](IAMPolicyExamples.md) IAM policy の例で指定する必要な権限をAWS userが持っている必要がある。

**Amazon ECS first run wizardの設定をプションを選択**

  
