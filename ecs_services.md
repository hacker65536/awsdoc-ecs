#Services
Amazon ECSはECSクラスタのtask definitionに指定されたinstanceの数を実行や維持を許可する。これをserviceという。もし、何かの理由でtaskが失敗や停止すると、Amazon ECS service スケジューラがtask definitionの別のinstanceを起動して置き換えをする。service内のtaskのdesired countを維持しようとする。

service内のtaskのdesired countを維持することに加えて、オプションでload balancerの後ろでserviceを実行できる。ロードバランサーはserviceによって関連付けられたtaskにトラフィックを分配する。

**Topics**
* [Service Concepts]
* [Service Definition Parameters]