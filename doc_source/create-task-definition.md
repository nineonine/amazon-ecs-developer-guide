# Creating a task definition using the new console<a name="create-task-definition"></a>

Create your task definitions using the new Amazon ECS console experience\. To make the task definition creation process as easy as possible, the console has default selections for many choices which we describe below\. There are also help panels available for most of the sections in the console which provide further context\.

**To create a new task definition \(New Amazon ECS console\)**

1. Open the new console at [https://console\.aws\.amazon\.com/ecs/v2](https://console.aws.amazon.com/ecs/v2)\.

1. In the navigation pane, choose **Task definitions**, **Create new task definition**\.

1. For **Task definition family**, specify a unique name for the task definition\.

1. For each container to define in your task definition, complete the following steps\.

   1. For **Name**, specify a name for the container\.

   1. For **Image URI**, specify the image to use to start a container\. Images in the Docker Hub registry are may be specified using the Docker Hub registry name only\. For example, if `amazonlinux:latest` is specified, the Amazon Linux container hosted on Docker Hub is used\. For all other repositories, specify the repository using either the `repository-url/image:tag` or `repository-url/image@digest` formats\.

   1. For **Essential container**, if your task definition has two or more containers defined, you may specify whether the container should be considered essential\. If a container is marked as *essential*, if that container stops then the task is stopped\. Each task definition must contain at least one essential container\.

   1. For **Container port** and **Protocol**, specify the port mapping to use for the container\. A port mapping allows the container to access ports on the host to send or receive traffic\. Choose **Add more port mappings** to specify additional container port mappings\.

   1. Expand the **Environment variables** section to specify environment variables to inject into the container\. You can specify environment variables either individually using key\-value pairs or in bulk by specifying an environment variable file hosted in an Amazon S3 bucket\. For information on how to format an environment variable file, see [Specifying environment variables](taskdef-envfiles.md)\.

   1. \(Optional\) Choose **Add more containers** to add additional containers to the task definition\. Choose **Next** once all containers have been defined\.

1. For **App environment**, choose **AWS Fargate \(serverless\)**, **Amazon EC2 instances**, or both\. Amazon ECS performs validation using this value to ensure the task definition parameters are valid for the infrastructure type\.

1. For **Operating system/Architecture**, choose the operating system and CPU architecture for the task\. 

   To run your task on a 64\-bit ARM architecture, select **Linux/ARM64**\. For more information, see [Runtime platform](task_definition_parameters.md#runtime-platform)\.

   To run your **AWS Fargate \(serverless\)** tasks on Windows containers, choose a supported Windows operating system\. For more information, see [Task Operating Systems](AWS_Fargate.md#fargate-task-os)\.

1. For **Task size**, specify the CPU and memory values to reserve for the task\. The CPU value is specified as vCPUs and memory is specified as GB\.

   For tasks hosted on Fargate, the following table shows the valid CPU and memory combinations\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html)

   For tasks hosted on Amazon EC2, supported task CPU values are between 128 CPU units \(0\.125 vCPUs\) and 10240 CPU units \(10 vCPUs\)\.
**Note**  
Task\-level CPU and memory parameters are ignored for Windows containers\.

1. Expand the **Container size** section to specify the amount \(in GB\) of memory to present to the container and the number of CPU units the Amazon ECS container agent will reserve for the container\. 

   If your container attempts to exceed the memory specified, the container is killed\. The total amount of memory reserved for all containers within a task must be lower than the task **Memory** value, if one is specified\.

   The total amount of CPU reserved for all containers within a task must be lower than the task\-level **CPU** value\.

   You can multiply the specified value by 1024 to determine the number of CPU units that are available per Amazon EC2 instance type\. For example, the value for a t3 nano instance is 2048\. For more information, see [Amazon EC2 Instances](http://aws.amazon.com/ec2/instance-types/) \.

1. \(Optional\) Expand the **Task roles, network mode** section to specify an IAM role to assign to the task\. A task IAM role provides permissions for the containers in a task to call AWS APIs\.

1. \(Optional\) The **Storage** section is used to expand the amount of ephemeral storage for tasks hosted on Fargate as well as add a data volume configuration for the task\.

   1. For **Amount**, to expand the available ephemeral storage beyond the default value of 20 GiB for your Fargate tasks, specify a value up to 200 GiB\.

1. \(Optional\) Choose **Add volume** to add a data volume configuration for the task\. For each data volume, complete the following steps\.

   1. For **Volume type**, choose **Bind mount**\.

   1. For **Volume name**, specify a name for the data volume\. The data volume name is used when creating a container mount point in a later step\.

   1. Expand the **Container mount points** section and choose **Add**\.

   1. For **Container**, choose the container for the mount point\.

   1. For **Source volume**, choose the data volume to mount to the container\.

   1. For **Container path**, specify the path on the container to mount the volume\.

   1. For **Read only**, specify whether to make the volume read only\.

   1. Choose **Add** to add additional mount points until each data volume defined in the task definition has a mount point defined\.

1. \(Optional\) Select the **Use log collection** option to specify a log configuration\. For each available log driver, there are log driver options to specify\. The default option sends container logs to CloudWatch Logs\. The other log driver options are configured using AWS FireLens\. For more information, see [Custom log routing](using_firelens.md)\.

   The following describes each container log destination in more detail\.
   + **Amazon CloudWatch** — Configure the task to send container logs to CloudWatch Logs\. The default log driver options are provided which creates a CloudWatch log group on your behalf\. To specify a different log group name, change the driver option values\.
   + **Amazon Kinesis Data Firehose** — Configure the task to send container logs to Kinesis Data Firehose\. The default log driver options are provided which sends logs to an Kinesis Data Firehose delivery stream\. To specify a different delivery stream name, change the driver option values\.
   + **Amazon Kinesis Data Streams** — Configure the task to send container logs to Kinesis Data Streams\. The default log driver options are provided which sends logs to an Kinesis Data Streams stream\. To specify a different stream name, change the driver option values\.
   + **Amazon OpenSearch Service** — Configure the task to send container logs to an OpenSearch Service domain\. The log driver options must be provided\. For more information, see [Forwarding logs to an Amazon OpenSearch Service domain](firelens-example-taskdefs.md#firelens-example-opensearch)\.
   + **Amazon S3** — Configure the task to send container logs to an Amazon S3 bucket\. The default log driver options are provided but you must specify a valid Amazon S3 bucket name\.

1. \(Optional\) Select the **Use trace collection** option to configure your tasks to route trace data from your application to AWS X\-Ray\. When this option is selected, Amazon ECS creates an AWS Distro for OpenTelemetry container sidecar which is preconfigured to send the trace data\. For more information, see [Collecting application trace data](trace-data.md)\.
**Important**  
When enabling trace collection, your task definition requires a task IAM role with the required permissions\. For more information, see [Required IAM permissions for AWS Distro for OpenTelemetry integration with AWS X\-Ray](trace-data.md#trace-data-iam)\. 

1. \(Optional\) Select the **Use metric collection** option to collect and send metrics for your tasks to either Amazon CloudWatch or Amazon Managed Service for Prometheus\. When this option is selected, Amazon ECS creates an AWS Distro for OpenTelemetry container sidecar which is preconfigured to send the application metrics\. For more information, see [Collecting application metrics](metrics-data.md)\.

   1. When **Amazon CloudWatch** is selected, your custom application metrics are routed to CloudWatch as custom metrics\. For more information, see [Exporting application metrics to Amazon CloudWatch](application-metrics-cloudwatch.md)\.
**Important**  
When exporting application metrics to Amazon CloudWatch, your task definition requires a task IAM role with the required permissions\. For more information, see [Required IAM permissions for AWS Distro for OpenTelemetry integration with Amazon CloudWatch](application-metrics-cloudwatch.md#application-metrics-cloudwatch-iam)\. 

   1. When **Amazon Managed Service for Prometheus \(Prometheus libraries instrumentation\)** is selected, your task\-level CPU, memory, network, and storage metrics and your custom application metrics are routed to Amazon Managed Service for Prometheus\. For **Workspace remote write endpoint**, specify the remote write endpoint URL for your Prometheus workspace\. For **Scraping target**, specify the host and port the AWS Distro for OpenTelemetry collector can use to scrape for metrics data\. For more information, see [Exporting application metrics to Amazon Managed Service for Prometheus](application-metrics-prometheus.md)\.
**Important**  
When exporting application metrics to Amazon Managed Service for Prometheus, your task definition requires a task IAM role with the required permissions\. For more information, see [Required IAM permissions for AWS Distro for OpenTelemetry integration with Amazon Managed Service for Prometheus](application-metrics-prometheus.md#application-metrics-prometheus-iam)\. 

   1. When **Amazon Managed Service for Prometheus \(OpenTelemetry instrumentation\)** is selected, your task\-level CPU, memory, network, and storage metrics and your custom application metrics are routed to Amazon Managed Service for Prometheus\. For **Workspace remote write endpoint**, specify the remote write endpoint URL for your Prometheus workspace\. For more information, see [Exporting application metrics to Amazon Managed Service for Prometheus](application-metrics-prometheus.md)\.
**Important**  
When exporting application metrics to Amazon Managed Service for Prometheus, your task definition requires a task IAM role with the required permissions\. For more information, see [Required IAM permissions for AWS Distro for OpenTelemetry integration with Amazon Managed Service for Prometheus](application-metrics-prometheus.md#application-metrics-prometheus-iam)\. 

1. \(Optional\) Expand the **Tags** section to add tags, as key\-value pairs, to the task definition\.

1. Choose **Next** to review the task definition\.

1. On the **Review and create** page, review each task definition section\. Choose **Edit** to make changes\. Once the task definition is complete, choose **Create** to register the task definition\.