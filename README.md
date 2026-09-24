# terraform-aws-ecs-app-worker

[![Lint Status](https://github.com/DNXLabs/terraform-aws-ecs-app-worker/workflows/Lint/badge.svg)](https://github.com/DNXLabs/terraform-aws-ecs-app-worker/actions)
[![LICENSE](https://img.shields.io/github/license/DNXLabs/terraform-aws-ecs-app-worker)](https://github.com/DNXLabs/terraform-aws-ecs-app-worker/blob/master/LICENSE)

Terraform-aws-ecs-app-worker is an AWS ECS Application Module for Workers without Application Load Balancer(ALB).

This module is designed to be used with `DNXLabs/terraform-aws-ecs` (https://github.com/DNXLabs/terraform-aws-ecs).

The following resources will be created:
 
 - Cloudwatch Metrics alarm - Provides a CloudWatch Metric Alarm resource.
   - High memory
   - High cpu
 - IAM roles - The cloudwatch event needs an IAM Role to run the ECS task definition. A role is created and a policy will be granted via IAM policy.
 - ECS task definition - A task definition is required to run Docker containers in Amazon ECS. Some of the parameters you can specify in a task definition include:
      - Image - Docker image to deploy.
           - Default value is "dnxsolutions/nginx-hello:latest"
      - CPU - Hard limit of the CPU for the container
           -  Default Value = 0
      - Memory - Hard memory of the container
           -  Default Value = 512
      - Name - Name of the ECS Service
      - Set log configuration
 - ECS Task-scheduler activated by cloudwatch events

In addition you have the option to create or not :

 - Simple Notification Service (SNS) topics - Alarm topics to create and alert on ECS service metrics. Leaving empty disables all alarms.
 - Cloudwatch Log Groups   
      - You can specify the number of days you want to retain log events in the specified log group. Possible values are: 1, 3, 5, 7, 14, 30, 60, 90, 120, 150, 180, 365, 400, 545, 731, 1827, and 3653.
      - Export to a S3 Bucket - Whether to mark the log group to export to an S3 bucket (needs the module terraform-aws-log-exporter (https://github.com/DNXLabs/terraform-aws-log-exporter) to be deployed in the account/region)

<!--- BEGIN_TF_DOCS --->

## Requirements

| Name | Version |
|------|---------|
| terraform | >= 0.13.0 |

## Providers

| Name | Version |
|------|---------|
| aws | n/a |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| alarm\_prefix | String prefix for cloudwatch alarms. (Optional, leave blank to use iam\_account\_alias) | `string` | `""` | no |
| alarm\_sns\_topics | Alarm topics to create and alert on ECS service metrics | `list` | `[]` | no |
| cloudwatch\_logs\_export | Whether to mark the log group to export to an S3 bucket (needs terraform-aws-log-exporter to be deployed in the account/region) | `bool` | `false` | no |
| cloudwatch\_logs\_retention | Specifies the number of days you want to retain log events in the specified log group. Possible values are: 1, 3, 5, 7, 14, 30, 60, 90, 120, 150, 180, 365, 400, 545, 731, 1827, and 3653. | `number` | `120` | no |
| cluster\_name | n/a | `string` | `"Name of existing ECS Cluster to deploy this app to"` | no |
| cpu | Hard limit for CPU for the container | `string` | `"0"` | no |
| deployment\_maximum\_percent | Deployment maximum percentage | `string` | `"100"` | no |
| deployment\_minimum\_healthy\_percent | Deployment minumum health percentage | `string` | `"0"` | no |
| desired\_count | Number of containers (tasks) to run | `number` | `1` | no |
| fargate\_spot | Set true to use FARGATE\_SPOT capacity provider by default (only when launch\_type=FARGATE) | `bool` | `false` | no |
| image | Docker image to deploy (can be a placeholder) | `string` | `"dnxsolutions/nginx-hello:latest"` | no |
| launch\_type | The launch type on which to run your service. The valid values are EC2 and FARGATE. Defaults to EC2. | `string` | `"EC2"` | no |
| log\_subscription\_filter\_destination\_arn | Destination for log subscription filter (required when log\_subscription\_filter\_enabled=true) | `string` | `""` | no |
| log\_subscription\_filter\_enabled | Enable cloudwatch log subscription filter | `bool` | `false` | no |
| log\_subscription\_filter\_filter\_pattern | Filter pattern for log subscription filter | `string` | `""` | no |
| log\_subscription\_filter\_role\_arn | Role to use for log subscription filter (required when log\_subscription\_filter\_enabled=true) | `string` | `""` | no |
| memory | Hard memory of the container | `string` | `"512"` | no |
| name | Name of your ECS service | `any` | n/a | yes |
| network\_mode | The Docker networking mode to use for the containers in the task. The valid values are none, bridge, awsvpc, and host. (REQUIRED IF 'LAUCH\_TYPE' IS FARGATE) | `any` | `null` | no |
| ordered\_placement\_strategy | Service level strategy rules that are taken into consideration during task placement. List from top to bottom in order of precedence. The maximum number of ordered\_placement\_strategy blocks is 5. | <pre>list(object({<br>    field      = string<br>    expression = string<br>  }))</pre> | `[]` | no |
| placement\_constraints | Rules that are taken into consideration during task placement. Maximum number of placement\_constraints is 10. | <pre>list(object({<br>    type       = string<br>    expression = string<br>  }))</pre> | `[]` | no |
| security\_groups | The security groups associated with the task or service | `any` | `null` | no |
| service\_role\_arn | Existing service role ARN created by ECS cluster module | `any` | n/a | yes |
| subnets | The subnets associated with the task or service. (REQUIRED IF 'LAUCH\_TYPE' IS FARGATE) | `any` | `null` | no |
| task\_role\_arn | Existing task role ARN created by ECS cluster module | `any` | n/a | yes |
| vpc\_id | VPC ID to deploy this app to | `any` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| aws\_cloudwatch\_log\_group\_arn | n/a |
| aws\_cloudwatch\_log\_group\_name | n/a |

<!--- END_TF_DOCS --->


## Authors

Module managed by [DNX Solutions](https://github.com/DNXLabs).

## License

Apache 2 Licensed. See [LICENSE](https://github.com/DNXLabs/terraform-aws-ecs-app-worker/blob/master/LICENSE) for full details.

<!-- BEGIN_TF_DOCS -->
## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [aws_iam_account_alias.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/iam_account_alias) | data source |
| [aws_region.current](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/region) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_alarm_prefix"></a> [alarm\_prefix](#input\_alarm\_prefix) | String prefix for cloudwatch alarms. (Optional, leave blank to use iam\_account\_alias) | `string` | `""` | no |
| <a name="input_alarm_sns_topics"></a> [alarm\_sns\_topics](#input\_alarm\_sns\_topics) | Alarm topics to create and alert on ECS service metrics | `list` | `[]` | no |
| <a name="input_cloudwatch_logs_export"></a> [cloudwatch\_logs\_export](#input\_cloudwatch\_logs\_export) | Whether to mark the log group to export to an S3 bucket (needs terraform-aws-log-exporter to be deployed in the account/region) | `bool` | `false` | no |
| <a name="input_cloudwatch_logs_retention"></a> [cloudwatch\_logs\_retention](#input\_cloudwatch\_logs\_retention) | Specifies the number of days you want to retain log events in the specified log group. Possible values are: 1, 3, 5, 7, 14, 30, 60, 90, 120, 150, 180, 365, 400, 545, 731, 1827, and 3653. | `number` | `120` | no |
| <a name="input_cluster_name"></a> [cluster\_name](#input\_cluster\_name) | n/a | `string` | `"Name of existing ECS Cluster to deploy this app to"` | no |
| <a name="input_command"></a> [command](#input\_command) | The command passed to the container, exec form (e.g. ["node", "dist/worker.js"]). Overrides the Docker image's default CMD. Leave unset to use the image's default command. | `list(string)` | `null` | no |
| <a name="input_cpu"></a> [cpu](#input\_cpu) | Hard limit for CPU for the container | `string` | `"0"` | no |
| <a name="input_deployment_maximum_percent"></a> [deployment\_maximum\_percent](#input\_deployment\_maximum\_percent) | Deployment maximum percentage | `string` | `"100"` | no |
| <a name="input_deployment_minimum_healthy_percent"></a> [deployment\_minimum\_healthy\_percent](#input\_deployment\_minimum\_healthy\_percent) | Deployment minumum health percentage | `string` | `"0"` | no |
| <a name="input_desired_count"></a> [desired\_count](#input\_desired\_count) | Number of containers (tasks) to run | `number` | `1` | no |
| <a name="input_efs_access_point_gid"></a> [efs\_access\_point\_gid](#input\_efs\_access\_point\_gid) | Owner gid of the access point root directory. | `number` | `0` | no |
| <a name="input_efs_access_point_path"></a> [efs\_access\_point\_path](#input\_efs\_access\_point\_path) | Root directory on the EFS filesystem for this service's access point. Defaults to /<name>. Set it explicitly when several services must share one directory - two services pointing at the same path see the same files. | `string` | `null` | no |
| <a name="input_efs_access_point_permissions"></a> [efs\_access\_point\_permissions](#input\_efs\_access\_point\_permissions) | POSIX permissions applied to the access point root directory. | `string` | `"755"` | no |
| <a name="input_efs_access_point_uid"></a> [efs\_access\_point\_uid](#input\_efs\_access\_point\_uid) | Owner uid of the access point root directory. The default of 0 makes the mount read-only for a container running as a non-root user; set it to that user's uid when the container has to write. | `number` | `0` | no |
| <a name="input_efs_mapping"></a> [efs\_mapping](#input\_efs\_mapping) | A map of EFS filesystem ids to container mount paths, e.g. { fs-0abc123 = "/opt/airflow/custom\_config" }. Leave empty for no EFS volumes. | `map(string)` | `{}` | no |
| <a name="input_fargate_spot"></a> [fargate\_spot](#input\_fargate\_spot) | Set true to use FARGATE\_SPOT capacity provider by default (only when launch\_type=FARGATE) | `bool` | `false` | no |
| <a name="input_image"></a> [image](#input\_image) | Docker image to deploy (can be a placeholder) | `string` | `"dnxsolutions/nginx-hello:latest"` | no |
| <a name="input_launch_type"></a> [launch\_type](#input\_launch\_type) | The launch type on which to run your service. The valid values are EC2 and FARGATE. Defaults to EC2. | `string` | `"EC2"` | no |
| <a name="input_log_subscription_filter_destination_arn"></a> [log\_subscription\_filter\_destination\_arn](#input\_log\_subscription\_filter\_destination\_arn) | Destination for log subscription filter (required when log\_subscription\_filter\_enabled=true) | `string` | `""` | no |
| <a name="input_log_subscription_filter_enabled"></a> [log\_subscription\_filter\_enabled](#input\_log\_subscription\_filter\_enabled) | Enable cloudwatch log subscription filter | `bool` | `false` | no |
| <a name="input_log_subscription_filter_filter_pattern"></a> [log\_subscription\_filter\_filter\_pattern](#input\_log\_subscription\_filter\_filter\_pattern) | Filter pattern for log subscription filter | `string` | `""` | no |
| <a name="input_log_subscription_filter_role_arn"></a> [log\_subscription\_filter\_role\_arn](#input\_log\_subscription\_filter\_role\_arn) | Role to use for log subscription filter (required when log\_subscription\_filter\_enabled=true) | `string` | `""` | no |
| <a name="input_memory"></a> [memory](#input\_memory) | Hard memory of the container | `string` | `"512"` | no |
| <a name="input_name"></a> [name](#input\_name) | Name of your ECS service | `any` | n/a | yes |
| <a name="input_network_mode"></a> [network\_mode](#input\_network\_mode) | The Docker networking mode to use for the containers in the task. The valid values are none, bridge, awsvpc, and host. (REQUIRED IF 'LAUCH\_TYPE' IS FARGATE) | `any` | `null` | no |
| <a name="input_ordered_placement_strategy"></a> [ordered\_placement\_strategy](#input\_ordered\_placement\_strategy) | Service level strategy rules that are taken into consideration during task placement. List from top to bottom in order of precedence. The maximum number of ordered\_placement\_strategy blocks is 5. | <pre>list(object({<br>    field      = string<br>    expression = string<br>  }))</pre> | `[]` | no |
| <a name="input_placement_constraints"></a> [placement\_constraints](#input\_placement\_constraints) | Rules that are taken into consideration during task placement. Maximum number of placement\_constraints is 10. | <pre>list(object({<br>    type       = string<br>    expression = string<br>  }))</pre> | `[]` | no |
| <a name="input_security_groups"></a> [security\_groups](#input\_security\_groups) | The security groups associated with the task or service | `any` | `null` | no |
| <a name="input_service_role_arn"></a> [service\_role\_arn](#input\_service\_role\_arn) | Existing service role ARN created by ECS cluster module | `any` | n/a | yes |
| <a name="input_ssm_variables"></a> [ssm\_variables](#input\_ssm\_variables) | Map of variables and SSM locations to add to the task definition | `map(string)` | `{}` | no |
| <a name="input_static_variables"></a> [static\_variables](#input\_static\_variables) | Map of variables and static values to add to the task definition | `map(string)` | `{}` | no |
| <a name="input_subnets"></a> [subnets](#input\_subnets) | The subnets associated with the task or service. (REQUIRED IF 'LAUCH\_TYPE' IS FARGATE) | `any` | `null` | no |
| <a name="input_task_role_arn"></a> [task\_role\_arn](#input\_task\_role\_arn) | Existing task role ARN created by ECS cluster module | `any` | n/a | yes |
| <a name="input_ulimits"></a> [ulimits](#input\_ulimits) | Container ulimit settings. This is a list of maps, where each map should contain "name", "hardLimit" and "softLimit" | <pre>list(object({<br>    name      = string<br>    hardLimit = number<br>    softLimit = number<br>  }))</pre> | `null` | no |
| <a name="input_vpc_id"></a> [vpc\_id](#input\_vpc\_id) | VPC ID to deploy this app to | `any` | n/a | yes |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_aws_cloudwatch_log_group_arn"></a> [aws\_cloudwatch\_log\_group\_arn](#output\_aws\_cloudwatch\_log\_group\_arn) | n/a |
| <a name="output_aws_cloudwatch_log_group_name"></a> [aws\_cloudwatch\_log\_group\_name](#output\_aws\_cloudwatch\_log\_group\_name) | n/a |
<!-- END_TF_DOCS -->