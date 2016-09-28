# Summary

* [Introduction](README.md)
* [What is Amazon ECS?](Welcome.md)
* [Setting Up](get-set-up-for-amazon-ecs.md)
* [Docker Basics](docker-basics.md)
* [Getting Started](ECS_GetStarted.md)
* [Cleaning Up](ECS_CleaningUp.md)
* [Clusters](ECS_clusters.md)
   * [Creating a Cluster](create_cluster.md)
   * [Scaling a Cluster](scale_cluster.md)
   * [Deleting a Cluter](delete_cluster.md)
* [Continer Instances](ECS_instances.md)
   * Container Instance AMIs
     * Amazon ECS-optimized AMI
   * Launching a Container instance
   * Connect to your Container Instance
   * [CloudWatch Logs](using_cloudwatch_logs.md)
   * [Managing Container Instances Remotely](ec2-run-command.md)
   * [Starting a Task at Container Instance Launch Time](start_task_at_launch.md)
   * [Deregister Container Instance](deregister_container_instance.md)
* [Container Agent](ECS_agent.md)
  * [Installing the Amazon ECS Container Agent](ecs-agent-install.md)
  * [Container Agent Versions](container_agent_versions.md)
  * [Updating the Amazon ECS Container Agent](ecs-agent-update.md)
  * [Amazon ECS Container Agent Configuration](ecs-agent-config.md)
  * [Private Registry Athentication](private-auth.md)
  * [Amazon ECS Container Agent Introspection](ecs-agent-introspection.md)
  * [HTTP Proxy Configuration](http_proxy_config.md)
* [Task Definitions](task_definitions.md)
  * [Application Architecture](application_architecture.md)
  * [Createing a Task Definition](create-task-definition.md)
  * [Using a Docker Image in a Task Definition](using_docker_image.md)
  * [Task Definition Parameters](task_definition_parameters.md)
  * [Using Data Volumes in Tasks](using_data_volumes.md)
  * [Using the awslogs Log Driver](using_awslogs.md)
  * [Example Task Definitions](example_task_definitions.md)
  * [Deregistering Task Definitions](deregister-task-definitions.md)
* [Scheduling Amazon ECS Tasks](scheduling_tasks.md)
  * [Running Tasks]
  * [Task Life Cycle]
* [Services]
  * [Service Definition Parameters]
* [IAM Policeis, Roles, and Permissions](IAM_policies.md)
  * [Policy Structure]
  * [Supported Resorce-Level Permissions]
  * [Creating IAM Policies]
  * [Managed Policies]
  * [Amazon ECS Container Instance IAM Role]
  * [Amazon ECS Service Scheduler IAM Role]
  * [Amazon EC2 Container Service Task Role]
  * [IAM Roles for Tasks](task-iam-roles.md)
  * [Amazon ECS IAM Policy Examples]
