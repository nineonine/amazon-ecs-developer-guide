# Windows task definitions<a name="windows_task_definitions"></a>

Windows containers and container instances can't support all the task definition parameters that are available for Linux containers and container instances\. For some parameters, they aren't supported at all, and others behave differently on Windows than they do on Linux\.

## Windows task definition parameters<a name="windows_task_definition_params"></a>

The following list explains which parameters aren't supported or behave differently on Windows containers than they do with Linux containers\. For more information about these parameters as they relate to Amazon ECS, see [Task definition parameters](task_definition_parameters.md)\.

`taskRoleArn`  
Supported: Yes  
IAM roles for tasks on Windows require that the `-EnableTaskIAMRole` option is set when you launch the Amazon ECS\-optimized Windows Server AMI\. Your containers must also run some configuration code in order to take advantage of the feature\. For more information, see [Windows IAM roles for tasks](windows_task_IAM_roles.md)\.

`networkMode`  
Supported: Yes  
When you register a task definition with Windows containers, you must use `default` or `awsvpc` network mode\. 

`containerDefinitions`  
Supported: Yes  
Additional notes: Not all container definition parameters are supported\. Review the following list for individual parameter support\.    
`portMappings`  
Supported: Limited  
Port mappings on Windows use the `NetNAT` gateway address rather than `localhost`\. There is no loopback for port mappings on Windows, so you can't access a container's mapped port from the host itself\.   
`cpu`  
Supported: Yes  
Amazon ECS treats this parameter in the same manner that it does for Linux containers: if you provide 500 CPU shares to a container, then 500 CPU shares is removed from the available resources on the container instance when the task is placed\. However, on a Windows container instance, the CPU limit is enforced as an absolute limit, or a quota\. Windows containers only have access to the specified amount of CPU that is described in the task definition\.  
`disableNetworking`  
Supported: No  
`dnsServers`  
Supported: No  
`dnsSearchDomains`  
Supported: No  
`dockerSecurityOptions`  
Supported: No  
`extraHosts`  
Supported: No  
`links`  
Supported: No  
`mountPoints`  
Supported: Limited  
Windows containers can mount whole directories on the same drive as `$env:ProgramData`\. Windows containers can't mount directories on a different drive, and mount point can't be across drives\.  
`linuxParameters`  
Supported: No  
`privileged`  
Supported: No  
`readonlyRootFilesystem`  
Supported: No  
`user`  
Supported: No  
`ulimits`  
Supported: No

`volumes`  
Supported: Yes    
`name`  
Supported: Yes  
`dockerVolumeConfiguration`  
Supported: No  
`host`  
Supported: Limited  
Windows containers can mount whole directories on the same drive as `$env:ProgramData`\. Windows containers can't mount directories on a different drive, and mount point can't be across drives\. For example, you can mount `C:\my\path:C:\my\path` and `D:\:D:\`, but not `D:\my\path:C:\my\path` or `D:\:C:\my\path`\.  
`FSxWindowsFileServerVolumeConfiguration`  
Supported: Yes  
This parameter is specified when you are using the [FSx for Windows File Server](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html) file system for task storage\. For more information, see [FSx for Windows File Server volumes](wfsx-volumes.md)\.

`cpu`  
Supported: No  
Task\-level CPU is ignored for Windows containers\. We recommend specifying container\-level CPU for Windows containers\.

`memory`  
Supported: No  
Task\-level memory is ignored for Windows containers\. We recommend specifying container\-level memory for Windows containers\.

`proxyConfiguration`  
Supported: No

`ipcMode`  
Supported: No

`pidMode`  
Supported: No

## Windows sample task definitions<a name="windows_sample_task_defs"></a>

The following is a sample task definition to help you get started with Windows containers on Amazon ECS\.

**Example Amazon ECS Console Sample Application for Windows**  
The following task definition is the Amazon ECS console sample application that is produced in the first\-run wizard for Amazon ECS; it has been ported to use the `microsoft/iis` Windows container image\.  

```
{
  "family": "windows-simple-iis",
  "containerDefinitions": [
    {
      "name": "windows_sample_app",
      "image": "mcr.microsoft.com/windows/servercore/iis",
      "cpu": 1024,
      "entryPoint":["powershell", "-Command"],
      "command":["New-Item -Path C:\\inetpub\\wwwroot\\index.html -Type file -Value '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p>'; C:\\ServiceMonitor.exe w3svc"],
      "portMappings": [
        {
          "protocol": "tcp",
          "containerPort": 80
        }
      ],
      "memory": 1024,
      "essential": true
    }
  ],
  "networkMode": "awsvpc",
  "memory": "1024",
  "cpu": "1024"
}
```