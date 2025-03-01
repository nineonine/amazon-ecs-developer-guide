# Amazon ECS task definitions<a name="task_definitions"></a>

A task definition is required to run Docker containers in Amazon ECS\. The following are some of the parameters you can specify in a task definition:
+ The Docker image to use with each container in your task
+ How much CPU and memory to use with each task or each container within a task
+ The launch type to use, which determines the infrastructure on which your tasks are hosted
+ The Docker networking mode to use for the containers in your task
+ The logging configuration to use for your tasks
+ Whether the task should continue to run if the container finishes or fails
+ The command the container should run when it is started
+ Any data volumes that should be used with the containers in the task
+ The IAM role that your tasks should use

You can define multiple containers in a task definition\. The parameters that you use depend on the launch type you choose for the task\. Not all parameters are valid\. For more information about the parameters available and which launch types they are valid for in a task definition, see [Task definition parameters](task_definition_parameters.md)\.

Your entire application stack does not need to be on a single task definition, and in most cases it should not\. Your application can span multiple task definitions\. You can do this by combining related containers into their own task definitions, each representing a single component\. For more information, see [Application architecture](application_architecture.md)\.

**Topics**
+ [Application architecture](application_architecture.md)
+ [Creating a task definition using the new console](create-task-definition.md)
+ [Creating a task definition using the classic console](create-task-definition-classic.md)
+ [Task definition parameters](task_definition_parameters.md)
+ [Amazon ECS launch types](launch_types.md)
+ [Working with GPUs on Amazon ECS](ecs-gpu.md)
+ [Working with inference workloads on Amazon ECS](ecs-inference.md)
+ [Working with 64\-bit ARM workloads on Amazon ECS](ecs-arm64.md)
+ [Using data volumes in tasks](using_data_volumes.md)
+ [Managing container swap space](container-swap.md)
+ [Amazon ECS task networking](task-networking.md)
+ [Using the awslogs log driver](using_awslogs.md)
+ [Custom log routing](using_firelens.md)
+ [Private registry authentication for tasks](private-auth.md)
+ [Specifying sensitive data](specifying-sensitive-data.md)
+ [Specifying environment variables](taskdef-envfiles.md)
+ [Example task definitions](example_task_definitions.md)
+ [Updating a task definition using the new console](update-task-definition-console-v2.md)
+ [Updating a task definition using the classic console](update-task-definition.md)
+ [Deregistering a task definition revision](deregister-task-definition.md)