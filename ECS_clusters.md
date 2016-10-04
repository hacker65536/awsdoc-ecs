# Amazon ECS Clusters

**Topics**
* [Cluster Concepts](ECS_clusters.md#cluster_concepts)
* C[reating a Cluster](create_cluster.md)
* [Scaling a Cluster](scale_cluster.md)
* [Deleting a Cluster](delete_cluster.md)

##Cluster Concepts
* クラスタは複数の違う container instance typesを含みことが出来る
* クラスタはリージョン依存である
* container instanceは一度に一つのクラスタに所属できる
* 特定のクラスタへのアクセスを制限や許可できるカスタムの IAM policyを作ることが出来る。詳細は[Amazon ECS IAM Policy Examples](IAMPolicyExamples.md)の[Clusters](IAMPolicyExamples.md#IAM_cluster_policies)セクションを参照