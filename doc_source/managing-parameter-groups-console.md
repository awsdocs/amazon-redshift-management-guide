# Managing parameter groups using the console<a name="managing-parameter-groups-console"></a>

 You can view, create, modify, and delete parameter groups on the Amazon Redshift console\. 

You can view any of your parameter groups to see a summary of the values for parameters and workload management \(WLM\) configuration\. Group parameters appear on the **Parameters** tab, and **Workload queues** appear on the **Workload Management** tab\. 

## Creating a parameter group<a name="parameter-group-create"></a>

If you want to set parameter values that are different from the default parameter group, you can create your own parameter group, 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="parameter-group-create-parameters"></a>

**To create a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Workload management** to display the **Workload management** page\. 

1. Choose **Create** to display the **Create parameter group** window\. 

1. Enter a value for **Parameter group name** and **Description**\. 

1. Choose **Create** to create the parameter group\. 

### Original console<a name="parameter-group-create-parameters-originalconsole"></a><a name="parameter-group-create-task"></a>

**To create a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Parameter Groups**\.

1. On the **Parameter Groups** page, choose **Create Cluster Parameter Group**\.

1. In the **Create Cluster Parameter Group** dialog box, choose a parameter group family\. Then enter a parameter group name and a parameter group description\. For more information about naming constraints for parameter groups, see [Quotas and limits in Amazon Redshift](amazon-redshift-limits.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-create-10.png)

1. Choose **Create**\.

## Modifying a parameter group<a name="parameter-group-modify"></a>

 You can modify parameters to change the parameter settings and WLM configuration properties\. 

**Note**  
You can't modify the default parameter group\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="parameter-group-modify-parameters"></a>

**To modify a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Workload management** to display the **Workload management** page\. 

1. Choose the parameter group that you want to modify to display the details page, with tabs for **Parameters** and **Workload management**\. 

1. Choose the **Parameters** tab to view the current parameter settings\. 

1. Choose **Edit parameters** to enable changing settings for these parameters: 
   + `auto_analyze`
   + `datestyle`
   + `enable_user_activity_logging`
   + `extra_float_digits`
   + `max_concurrency_scaling_clusters`
   + `max_cursor_result_set_size`
   + `query_group`
   + `require_ssl`
   + `search_path`
   + `statement_timeout`
   + `use_fips_ssl`

   For more information about these parameters, see [Amazon Redshift parameter groups](working-with-parameter-groups.md)\. 

1. Enter your changes and then choose **Save** to update the parameter group\. 

**To modify the WLM configuration for a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Workload management** to display the **Workload management** page\. 

1. Choose the parameter group that you want to modify to display the details page with tabs for **Parameters** and **Workload management**\. 

1. Choose the **Workload management** tab to view the current WLM configuration\. 

1. Choose **Edit workload queues** to edit the WLM configuration, 

1. \(Optional\) Select **Enable short query acceleration** to enable short query acceleration \(SQA\)\.

   When you enable SQA, **Maximum run time for short queries \(1 to 20 seconds\)** is set to **Dynamic** by default\. To set the maximum runtime to a fixed value, choose a value of 1–20\.

1. Do one or more of the following to modify the queue configuration: 
   + Choose **Switch WLM mode** to choose between **Automatic WLM** and **Manual WLM**\.

     With **Automatic WLM**, the **Memory** and **Concurrency on main** values are set to **auto**\.
   + To create a queue, choose **Edit workload queues**, then choose **Add Queue**\.
   + To modify a queue, change property values in the table\. Depending on the type of queue, properties can include the following:
     + **Queue name** can be changed\. 
     + **Memory \(%\)**
     + **Concurrency on main** cluster
     + **Concurrency scaling mode** can be **off** or **auto**
     + **Timeout \(ms\)**
     + **User groups**
     + **Query groups**

     For more information about these properties, see [Properties for the wlm\_json\_configuration parameter](workload-mgmt-config.md#wlm-json-config-properties)\.
**Important**  
If you change a queue name, the `QueueName` dimension value of WLM queue metrics \(such as, WLMQueueLength, WLMQueueWaitTime, WLMQueriesCompletedPerSecond, WLMQueryDuration, WLMRunningQueries, and so on\) also changes\. So, if you change the name of a queue, you might need to change CloudWatch alarms you have set up\. 
   + To change the order of queues, choose the **Up** and **Down** arrow buttons\. 
   + To delete a queue, choose **Delete** in the queue's row in the table\.

1. \(Optional\) Select **Defer dynamic changes until reboot** to have the changes applied to clusters after their next reboot\.
**Note**  
Some changes require a cluster reboot regardless of this setting\. For more information, see [WLM dynamic and static properties](workload-mgmt-config.md#wlm-dynamic-and-static-properties)\.

1. Choose **Save**\.

### Original console<a name="parameter-group-modify-parameters-originalconsole"></a><a name="parameter-group-modify-task"></a>

**To modify parameters in a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

   In the navigation pane, choose **Parameter Groups**\.

1. On the **Parameter Groups** page, in the parameter group list, select the row of the parameter group that you want to modify\.

1. To edit the parameters other than the WLM configuration parameter, choose **Edit Parameters**\.

   The **Parameters** tab opens and enables you to update parameters in the parameter group\. You can update values for parameters such as the following:
   + `auto_analyze`
   + `datestyle`
   + `enable_user_activity_logging`
   + `extra_float_digits`
   + `force_acm`
   + `max_concurrency_scaling_clusters`
   + `query_group`
   + `require_ssl`
   + `search_path`
   + `statement_timeout`
   + `use_fips_ssl`

1. In the **Value** box that corresponds to the parameter you want to modify, enter a new value\. For more information about these parameters, see [Amazon Redshift parameter groups](working-with-parameter-groups.md)\.

1. Choose **Save Changes**\.
**Note**  
If you modify these parameters in a parameter group that is already associated with a cluster, reboot the cluster for the changes to be applied\. For more information, see [Rebooting a cluster](managing-clusters-console.md#reboot-cluster)\. <a name="parameter-group-modify-wlm-task"></a>

**To modify the WLM configuration in a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Workload management**\.

1. For **Parameter groups,** choose the parameter group that you want to modify\.
**Note**  
You can't modify the default parameter group\.

1. Choose **Edit** 

1. \(Optional\) Select **Enable short query acceleration** to enable short query acceleration \(SQA\)\.

   When you enable SQA, ** Maximum run time for short queries \(1 to 20 seconds\)** is set to **Dynamic** by default\. To set the maximum runtime to a fixed value, choose a value of 1–20\.

1. Do one or more of the following to modify the queue configuration: 
   + Choose **Switch WLM mode** to choose between **Auto WLM** and **Manual WLM**\.

     With **Auto WLM**, the **Memory** and **Concurrency on main** values are set to **auto**\.
   + To create a queue, choose **Add Queue**\.
   + To modify the **Max Concurrency Scaling clusters** parameter, choose **Edit** next to the current value that is displayed\.
   + To modify a queue, change property values in the table\. Depending on the type of queue, properties can include the following:
     + **Memory \(%\)**
     + **Concurrency on main** cluster
     + **Concurrency Scaling mode** can be **off** or **auto**
     + **Timeout \(ms\)**
     + **User groups**
     + **Query groups**
   + To change the order of queues, choose the **Up** and **Down** arrow buttons in the table\.
   + To delete a queue, choose **Delete** in the queue's row in the table\.

1. \(Optional\) Select **Defer dynamic changes until reboot** to have the changes applied to clusters after their next reboot\.
**Note**  
Some changes require a cluster reboot regardless of this setting\. For more information, see [WLM dynamic and static properties](workload-mgmt-config.md#wlm-dynamic-and-static-properties)\.

1. Choose **Save**\.

## Creating or modifying a query monitoring rule using the console<a name="parameter-group-modify-qmr-console"></a>

You can use the Amazon Redshift console to create and modify WLM query monitoring rules\. Query monitoring rules are part of the WLM configuration parameter for a parameter group\. For more information, see [WLM query monitoring rules](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-wlm-query-monitoring-rules.html)\. 

When you create a rule, you define the rule name, one or more predicates, and an action\. 

When you save WLM configuration that includes a rule, you can view the JSON code for the rule definition as part of the JSON for the WLM configuration parameter\. 

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="parameter-group-qmr-rules"></a>

**To create a query monitoring rule**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Workload management** to display the **Workload management** page\. 

1. Choose the parameter group that you want to modify to display the details page with tabs for **Parameters** and **Workload management**\. 

1. Choose the **Workload management** tab, and choose **Edit workload queues** to edit the WLM configuration,

1. Add a new rule either by using a predefined template or from scratch\. 

   To use a predefined template, do the following: 

   1. Choose **Add rule from template** in the **Query monitoring rules** group\. The list of rule templates is displayed\. 

   1. Choose one or more rule templates\. When you choose **Save**, WLM creates one rule for each template that you choose\. 

   1. Enter or confirm values for the rule, including **Rule names**, **Predicates** and **Actions**\. 

   1. Choose **Save**\. 

   To add a new rule from scratch, do the following:

   1. To add additional predicates, choose **Add predicate**\. You can have up to three predicates for each rule\. If all of the predicates are met, WLM triggers the associated action\. 

   1. Choose an **Action**\. Each rule has one action\.

   1. Choose **Save**\.

Amazon Redshift generates your WLM configuration parameter in JSON format and displays it in the **JSON** section\. 

### Original console<a name="parameter-group-qmr-rules-originalconsole"></a><a name="parameter-group-modify-qmr-task"></a>

**To create a query monitoring rule**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Workload management**\.

1. For **Parameter groups,** choose the parameter group that you want to modify\.
**Note**  
You can't modify the default parameter group\.

1. To edit the WLM configuration \(to add a rule\), choose **Edit**\. 

1. To create a new rule using a predefined template, in the **Rules for Queue 1** group, choose **Add Rule from Templates**\. The **Rule Templates** dialog box appears, as shown in the following screenshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-rule-template-dialog.png)

1. Choose one or more rule templates\. WLM creates one rule for each template you choose\. For this example, choose **Long running query with high I/O skew** and then choose **Select**\.

   A new rule appears with two predicates, as shown in the following screenshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-add-rule-template.png)

1. Enter a value for **Rule name**\. The name can be up to 32 alphanumeric characters and must not contain spaces or quotation mark characters\. For this example, enter **HighIOskew**\.

1. \(Optional\) Modify the rule predicates\.

1. Choose a value for **Action**\. Each rule has one action\. For this example, choose **Hop**\. Hop terminates the query and WLM routes the query to the next matching queue, if one is available\. 

1. Choose **Save**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-collapse-queue.png)

1. To modify the rules for a queue, choose **Edit**\.

1. To add a new rule from scratch, choose **Add Custom Rule**\. You can have a maximum of five rules per queue, and a total of eight rules for all queues\.

1. Enter a **Rule name**, for example **NestedLoop**\. 

1. Define a **Predicate** value\. Choose a predicate name, an operator, and a value\. For this example, choose **Nested loop join count \(rows\)**\. Leave the operator at greater than \( **>** \), and for the value type **1000**\. The following screen shot shows the new rule with one predicate\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-add-rule.png)

1. To add additional predicates, choose the add icon to the right of the predicates\. You can have up to three predicates per rule\. If all of the predicates are met, WLM triggers the associated action\. 

1. Choose an **Action**\. Each rule has one action\. For this example, accept the default action, **Log**\. The Log action writes a record to the [STL\_WLM\_RULE\_ACTION](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_WLM_RULE_ACTION.html) system table and leaves the query running in the queue\. 

1. Choose **Done Editing**\. The queue details collapse\.

1. Choose **Save**\.

Amazon Redshift generates your WLM configuration parameter in JSON format and displays the JSON in a window at the bottom of the screen, as shown in the following screenshot\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-json.png)

## Deleting a parameter group<a name="parameter-group-delete"></a>

You can delete a parameter group if you no longer need it and it is not associated with any clusters\. You can only delete custom parameter groups\.

**Note**  
A new console is available for Amazon Redshift\. Choose either the **New console** or the **Original console** instructions based on the console that you are using\. The **New console** instructions are open by default\.

### New console<a name="parameter-group-delete-parameters"></a>

**To delete a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. On the navigation menu, choose **CONFIG**, then choose **Workload management** to display the **Workload management** page\. 

1. For **Parameter groups,** choose the parameter group that you want to modify\.
**Note**  
You can't delete the default parameter group\.

1. Choose **Delete** and confirm that you want to delete the parameter group\. 

### Original console<a name="parameter-group-delete-parameters-originalconsole"></a>

To delete a parameter group\. <a name="parameter-group-delete-task"></a>

**To delete a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Parameter Groups**\.

1. Select the row of the parameter group that you want to delete, and then choose **Delete**\. 
**Note**  
You can't delete the default parameter group\.

1. In the **Delete Cluster Parameter Groups** dialog box, choose **Continue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-delete.png)

## Associating a parameter group with a cluster<a name="parameter-group-associate"></a>

When you launch a cluster, you must associate it with a parameter group\. If you want to change the parameter group later, you can modify the cluster and choose a different parameter group\. For more information, see [Creating a cluster by using a launch cluster](managing-clusters-console.md#create-cluster-task) and [To modify a cluster](managing-clusters-console.md#modify-cluster-task)\. 