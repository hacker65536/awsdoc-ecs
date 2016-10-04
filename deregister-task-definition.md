#Deregistering Task Definitions
もしAmazon ECSにあるtask definitionはもう必要ではないと判断したならば、task definitionの登録解除を行うことが出来る。そうするとtaskの実行やserviceのアップデートをするときに、ListTaskDefinition APIコールや、コンソールで表示されなくなる。

task definitionの登録解除を行うと、直ちに__INACTIVE__にマークされる。INACTIVEを参照している既存のtaskとserviceは中断することなく実行を継続する、またINACTIVEを参照する既存のserviceはserviceのdesired countの変更によってスケールアップやダウンを行う。

INACTIVEのtask definitionを使用して新しいtaskの実行や新しいserviceを作ることはできない。またINACTIVEのtask definition参照している既存のserviceをアップデートできない。(しかし、まだ登録解除による制限が適応されないと10分windowに残る場合ある)

**task definitionの解除**
1. ECSコンソールを開く
2. ナビゲーションバーからtask definitionのコンテナがあるregionを選択
3. ナビゲーションパネルから**Task Definitions**を選択
4. **Task Definitions**ページにある登録解除したいtask definition(一つ以上のリビジョンを含む)の名前を選択する
5. **Task Definition name**ページで解除したいリビジョンの横のチェックボックスを選択
6. **Actions**を選択し、**Deregister**を選択
7. **Deregister Task definition**ウィンドウにある情報を確認し、**Deregister**を選択し完了