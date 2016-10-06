#Amazon ECS Troubleshooting
load balancers, tasks,servicesやcontainer instancesのトラブルシューティングが必要かもしれません。このチャプターはAmazon ECS コンソールにあるservice event logやcontainer instance上のDocker daemon、Amazone ECS container agentから診断情報を見つけることの助けになるだろう。

**Topcis**
* [Checking Stopped Tasks for Errors](#Checking Stoped Tasks for Errors)
* [Service Event Messages]
* [Troubleshooting Service Load Balancers]
* [Enabling Docker Debug Output]
* [Amazon ECS Log File Locations]
* [Agent introspection Diagnostics]
* [Docker Diagnostics]
* [API failures Error Messages]

##Checking Stoped Tasks for Errors
taskを開始時にtroubleに見舞われるとtaskはエラーによって停止するだろう(例えば、taskを実行しそのtaskがPENDING状態となり消える)。Amazon ECSコンソールで停止したtaskのエラーの表示やエラーメッセージの確認が出来る。

**errorによって停止したtaskをcheckする**
1. Amazon ECSコンソールを開く
2. **Cluster**ページで常駐している停止したtaskがあるclusterを選択
3. **Cluster:clustername**ページで**Tasks**タブを選びtaskを見る
4. **Desired task status**テーブルヘッダーで、**Stopped**を選択して停止のスタックを見る、そしてinspectしたい停止したtaskを選択する。直近の停止したtaskは最初にリストされる。
5. **Details**セクションで**Stopped reason**フィールドでtaskが停止した理由を見る。  
 [fig]  
 いくつかの理由は以下で説明する  
 
 Task failed ELB health checks in (elb elb-name)  
 
 カレントtaskはtask serviceによって関連付けられたロードバランサーのためのELB health checkに失敗した。詳細は[Troubleshooting Service Load Balancers](#troubleshoot-service-load-balancers)  
 
 Scaling activity initiated by (deployment deployment-id)  

 安定サービスのdesired countを減らす時に、desired numberに達しているいくつかのtaskは停止する必要がある。downscaling serviceによって停止したtaskはこの理由となる。  
 
 Host EC2(instance id)stopped/terminated  
  
 もしtask起動しているcontainer instanceの停止や終了するとtaskはこの理由を提示する。
  
 Container instance deregistration forced by user  

 もしtaskを起動しているcontainer instanceを強制的に登録解除をしたら、この理由になる。
 
 Essential container in task exited  
 
 もしtask definitions でessentialとマークされたコンテナが終了した場合、taskを停止する。essential containerの終了が原因でtaskが停止した場合、コンテナがなぜ停止したかの診断情報は[Step6]()で提供できる。
 
6. もし停止したコンテナがあれば、containerを展開して、**Status reason**行でtaskのstateが何によって変わったか確認できる。  
 前の例で、container image 名が見つけられない場合はimage名のスペルミスがあるかもしれません。
 もし、この方法で十分な情報を取得出来なければ、container instanceにsshで接続しDocker containerの内部を検査することが出来る。詳細は[Inspect Docker Containers](#docker-inspect)

##Service Event Messages


 

