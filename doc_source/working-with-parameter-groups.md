# Amazon Redshift parameter groups<a name="working-with-parameter-groups"></a>

## Overview<a name="working-with-parameter-groups-overview"></a>

 In Amazon Redshift, you associate a *parameter group* with each cluster that you create\. The parameter group is a group of parameters that apply to all of the databases that you create in the cluster\. These parameters configure database settings such as query timeout and datestyle\. 

## About parameter groups<a name="about-parameter-groups"></a>

Each parameter group has several parameters to configure settings for the database\. The list of available parameters depends on the parameter group family to which the parameter group belongs\. The *parameter group family* is the version of the Amazon Redshift engine to which the parameters in the parameter group apply\. The format of the parameter group family name is `redshift-version` where *version* is the engine version\. For example, the current version of the engine is `redshift-1.0`\. 

Amazon Redshift provides one default parameter group for each parameter group family\. The default parameter group has preset values for each of its parameters, and it cannot be modified\. The format of the default parameter group name is `default.parameter_group_family`, where *parameter\_group\_family* is the version of the engine to which the parameter group belongs\. For example, the default parameter group for the `redshift-1.0` version is named `default.redshift-1.0`\. 

**Note**  
 At this time, `redshift-1.0` is the only version of the Amazon Redshift engine\. Consequently, `default.redshift-1.0` is the only default parameter group\. 

If you want to use different parameter values than the default parameter group, you must create a custom parameter group and then associate your cluster with it\. Initially, the parameter values in a custom parameter group are the same as in the default parameter group\. The initial `source` for all of the parameters is `engine-default` because the values are preset by Amazon Redshift\. After you change a parameter value, the `source` changes to `user` to indicate that the value has been modified from its default value\. 

**Note**  
The Amazon Redshift console does not display the `source` of each parameter\. You must use the Amazon Redshift API, the AWS CLI, or one of the AWS SDKs to view the `source`\.

For parameter groups that you create, you can modify a parameter value at any time, or you can reset all parameter values to their defaults\. You can also associate a different parameter group with a cluster\. In some cases, you might modify parameter values in a parameter group that is already associated with a cluster or associate a different parameter group with a cluster\. In these cases, you might need to restart the cluster for the updated parameter values to take effect\. If the cluster fails and is restarted by Amazon Redshift, your changes are applied at that time\. Changes aren't applied if your cluster is restarted during maintenance\. For more information, see [WLM dynamic and static properties](workload-mgmt-config.md#wlm-dynamic-and-static-properties)\.

## Default parameter values<a name="default-param-group-values"></a>

The following table shows the default parameter values at a glance with links to more in\-depth information about each parameter\. These are the default values for the `redshift-1.0` parameter group family\. 


| Parameter name | Value | More information | 
| --- | --- | --- | 
|  auto\_analyze  |  true  |  [auto\_analyze](https://docs.aws.amazon.com/redshift/latest/dg/r_auto_analyze.html) in the Amazon Redshift Database Developer Guide  | 
|  datestyle  |   ISO, MDY   |  [datestyle](https://docs.aws.amazon.com/redshift/latest/dg/r_datestyle.html) in the Amazon Redshift Database Developer Guide  | 
|  enable\_user\_activity\_logging  |   false   |  [Database audit logging](db-auditing.md) in this guide  | 
|  extra\_float\_digits  |  0  |  [extra\_float\_digits](https://docs.aws.amazon.com/redshift/latest/dg/r_extra_float_digits.html) in the Amazon Redshift Database Developer Guide  | 
|  max\_concurrency\_scaling\_clusters  |  1  |  [max\_concurrency\_scaling\_clusters](https://docs.aws.amazon.com/redshift/latest/dg/r_max_concurrency_scaling_clusters.html) in the Amazon Redshift Database Developer Guide  | 
|  query\_group  |  default   |  [query\_group](https://docs.aws.amazon.com/redshift/latest/dg/r_query_group.html) in the Amazon Redshift Database Developer Guide  | 
|  require\_ssl  |  false  |  [Configuring security options for connections](connecting-ssl-support.md) in this guide  | 
|  search\_path  |   $user, public   |  [search\_path](https://docs.aws.amazon.com/redshift/latest/dg/r_search_path.html) in the Amazon Redshift Database Developer Guide  | 
|  statement\_timeout  |  0  |  [statement\_timeout](https://docs.aws.amazon.com/redshift/latest/dg/r_statement_timeout.html) in the Amazon Redshift Database Developer Guide  | 
|  wlm\_json\_configuration  |   \[\{"auto\_wlm":true\}\]   |  [Configuring workload management](workload-mgmt-config.md) in this guide | 
|  use\_fips\_ssl  |  false  |  Enable FIPS\-compliant SSL mode only if your system is required to be FIPS\-compliant\. | 

**Note**  
The `max_cursor_result_set_size` parameter is deprecated\. For more information about cursor result set size, see [ Cursor constraints](https://docs.aws.amazon.com/redshift/latest/dg/declare.html#declare-constraints) in the *Amazon Redshift Database Developer Guide*\.

You can temporarily override a parameter by using the `SET` command in the database\. The `SET` command overrides the parameter for the duration of your current session only\. In addition to the parameters listed in the preceding table, you can also temporarily adjust the slot count by setting `wlm_query_slot_count` in the database\. The `wlm_query_slot_count` parameter is not available for configuration in parameter groups\. For more information about adjusting the slot count, see [wlm\_query\_slot\_count](https://docs.aws.amazon.com/redshift/latest/dg/r_wlm_query_slot_count.html) in the *Amazon Redshift Database Developer Guide*\. For more information about temporarily overriding the other parameters, see [ Modifying the server configuration](https://docs.aws.amazon.com/redshift/latest/dg/t_Modifying_the_default_settings.html) in the *Amazon Redshift Database Developer Guide*\.

## Configuring parameter values using the AWS CLI<a name="configure-parameters-using-the-cli"></a>

 To configure Amazon Redshift parameters by using the AWS CLI, you use the `modify-cluster-parameter-group` command for a specific parameter group\. You specify the parameter group to modify in `parameter-group-name`\. You use the `parameters` parameter \(for the `modify-cluster-parameter-group` command\) to specify name/value pairs for each parameter that you want to modify in the parameter group\. 

**Note**  
There are special considerations when configuring the `wlm_json_configuration` parameter by using the AWS CLI\. The examples in this section apply to all of the parameters except `wlm_json_configuration`\. For more information about configuring `wlm_json_configuration` by using the AWS CLI, see [Configuring workload management](workload-mgmt-config.md)\. 

After you modify parameter values, you must reboot any clusters that are associated with the modified parameter group\. The cluster status displays `applying` for `ParameterApplyStatus` while the values are being applied, and then `pending-reboot` after the values have been applied\. After you reboot, the databases in your cluster begin to use the new parameter values\. For more information about rebooting clusters, see [Rebooting a cluster](managing-clusters-console.md#reboot-cluster)\. 

**Note**  
The `wlm_json_configuration` parameter contains some properties that are dynamic and do not require you to reboot associated clusters for the changes to be applied\. For more information about dynamic and static properties, see [WLM dynamic and static properties](workload-mgmt-config.md#wlm-dynamic-and-static-properties)\. 

Syntax

 The following syntax shows how to use the `modify-cluster-parameter-group` command to configure a parameter\. You specify *parameter\_group\_name* and replace both *parameter\_name* and *parameter\_value* with an actual parameter to modify and a value for that parameter\. If you want to modify more than one parameter at the same time, separate each parameter and value set from the next with a space\. 

```
aws redshift modify-cluster-parameter-group --parameter-group-name parameter_group_name --parameters ParameterName=parameter_name,ParameterValue=parameter_value
```

Example

 The following example shows how to configure the `statement_timeout` and `enable_user_activity_logging` parameters for the `myclusterparametergroup` parameter group\. 

**Note**  
 For readability purposes, the example is displayed on several lines, but in the actual AWS CLI this is one line\. 

```
aws redshift modify-cluster-parameter-group 
--parameter-group-name myclusterparametergroup 
--parameters ParameterName=statement_timeout,ParameterValue=20000 ParameterName=enable_user_activity_logging,ParameterValue=true
```