# Managing Parameter Groups Using the Console<a name="managing-parameter-groups-console"></a>

 You can view, create, modify, and delete parameter groups by using the Amazon Redshift console\. To initiate these tasks, use the buttons on the **Parameter Groups** page, as shown in the following screenshot\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-groups-menu-buttons.png)

You can expand any of the parameter groups in the list to see a summary of the values for parameters and workload management \(WLM\) configuration\. In the following screenshot, the parameter group called `custom-parameter-group` is expanded to show the summary of parameter values\. 

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-groups-list.png)

## Creating a Parameter Group<a name="parameter-group-create"></a>

You can create a parameter group if you want to set parameter values that are different from the default parameter group\.<a name="parameter-group-create-task"></a>

**To create a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Parameter Groups**\.

1. On the **Parameter Groups** page, choose **Create Cluster Parameter Group**\.

1. In the **Create Cluster Parameter Group** dialog box, choose a parameter group family, and then type a parameter group name and a parameter group description\. For more information about naming constraints for parameter groups, see [Limits in Amazon Redshift](amazon-redshift-limits.md)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-create-10.png)

1. Choose **Create**\.

## Modifying a Parameter Group<a name="parameter-group-modify"></a>

 You can modify parameters to change the parameter settings and WLM configuration properties\. 

**Note**  
You cannot modify the default parameter group\.<a name="parameter-group-modify-task"></a>

**To modify parameters in a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Parameter Groups**\.

1. On the **Parameter Groups** page, in the parameter group list, select the row of the parameter group that you want to modify\.

1. To edit the parameters other than the WLM configuration parameter, choose **Edit Parameters**\.

   The **Parameters** tab opens as shown in the following screenshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-modify-parameters.png)

1. In the **Value** box that corresponds to the parameter you want to modify, type a new value\. For more information about these parameters, see [Amazon Redshift Parameter Groups](working-with-parameter-groups.md)\.

1. Choose **Save Changes**\.
**Note**  
 If you modify these parameters in a parameter group that is already associated with a cluster, reboot the cluster for the changes to be applied\. For more information, see [Rebooting a Cluster](managing-clusters-console.md#reboot-cluster)\. <a name="parameter-group-modify-wlm-task"></a>

**To modify the WLM configuration in a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Workload management**\.

1. For **Parameter groups,** choose the parameter group that you want to modify\.
**Note**  
You cannot modify the default parameter group\.

1. To edit the WLM configuration, choose **Edit**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-modify-wlm.png)

1. To enable short query acceleration \(SQA\), choose **Enable short query acceleration**\.

1. When you enable SQA, ** Maximum run time for short queries \(1 to 20 seconds\)** is set to **Dynamic** by default\. To set the maximum run time to a fixed value, choose a value of 1–20

1. Do one or more of the following to modify the queue configuration: 
   + To create a queue, choose **Add Queue**\.
   + To modify a queue, change property values in the table\.
   + To change the order of queues, choose the **Up** and **Down** arrow buttons in the table\.
   + To delete a queue, choose **Delete** in the queue's row in the table\.

1. To have changes applied to associated clusters after their next reboot, choose **Apply dynamic changes after cluster reboot**\.
**Note**  
Some changes require a cluster reboot regardless of this setting\. For more information, see [WLM Dynamic and Static Properties](workload-mgmt-config.md#wlm-dynamic-and-static-properties)\.

1. Choose **Save**\.

## Creating or Modifying a Query Monitoring Rule Using the Console<a name="parameter-group-modify-qmr-console"></a>

You can use the AWS Management Console to create and modify WLM query management rules\. Query monitoring rules are part of the WLM configuration parameter for a parameter group\. For more information, see [WLM Query Monitoring Rules](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-wlm-query-monitoring-rules.html)\. 

When you create a rule, you define the rule name, one or more predicates, and an action\. 

When you save WLM configuration that includes a rule, you can view the JSON code for the rule definition as part of the JSON for the WLM configuration parameter\. <a name="parameter-group-modify-qmr-task"></a>

**To create a query monitoring rule**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Workload management**\.

1. For **Parameter groups,** choose the parameter group that you want to modify\.
**Note**  
You can't modify the default parameter group\.

1. To edit the WLM configuration, choose **Edit**\. 

1.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-modify-wlm.png)

1. Choose **Add queue**\. A new queue appears, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-add-queue.png)

1. To create a new rule using a predefined template, in the **Rules for Queue 1** group, choose **Add Rule from Templates**\. The **Rule Templates** dialog appears, as shown in the following screenshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-rule-template-dialog.png)

1. Choose one or more rule templates\. WLM creates one rule for each template you choose\. For this example, choose **Long running query with high I/O skew** and then choose **Select**\.

   A new rule appears with two predicates, as shown in the following screenshot\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-add-rule-template.png)

1. Type a **Rule name**\. The name can be up to 32 alphanumeric characters and must not contain spaces or quotation mark characters\. For this example, type **HighIOskew**\.

1. Optionally, modify the predicates\.

1. Choose an **Action**\. Each rule has one action\. For this example, choose **Hop**\. Hop terminates the query and WLM routes the query to the next matching queue, if one is available\. 

1. Choose **Save**\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-collapse-queue.png)

1. To modify the rules for a queue, choose **Edit**\.

1. To add a new queue from scratch, choose **Add Custom Rule**\. You can a maximum of five rules per queue, and a total of eight rules for all queues\.

1. Type a **Rule name**; for example, **NestedLoop**\. 

1. Define a **Predicate**\. Choose a predicate name, an operator, and a value\. For this example, choose **Nested loop join count \(rows\)**\. Leave the operator at greater than \( **>** \), and for the value type **1000**\. The following screen shot shows the new rule with one predicate\.   
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-add-rule.png)

1. To add additional predicates, choose the add icon to the right of the predicates\. You can have up to three predicates per rule\. If all of the predicates are met, WLM triggers the associated action\. 

1. Choose an **Action**\. Each rule has one action\. For this example, accept the default action, **Log**\. The Log action writes a record to the [STL\_WLM\_RULE\_ACTION](https://docs.aws.amazon.com/redshift/latest/dg/r_STL_WLM_RULE_ACTION.html) system table and leaves the query running in the queue\. 

1. Choose **Done Editing**\. The queue details collapse\.

1. Choose **Save**\.

1. Amazon Redshift generates your WLM configuration parameter in JSON format and displays the JSON in a window at the bottom of the screen, as shown in the following screenshot\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-qmr-json.png)

## Deleting a Parameter Group<a name="parameter-group-delete"></a>

You can delete a parameter group if you no longer need it and it is not associated with any clusters\. You can only delete custom parameter groups\.<a name="parameter-group-delete-task"></a>

**To delete a parameter group**

1. Sign in to the AWS Management Console and open the Amazon Redshift console at [https://console\.aws\.amazon\.com/redshift/](https://console.aws.amazon.com/redshift/)\.

1. In the navigation pane, choose **Parameter Groups**\.

1. Select the row of the parameter group that you want to delete, and then choose **Delete**\. 
**Note**  
You cannot delete the default parameter group\.

1. In the **Delete Cluster Parameter Groups** dialog box, choose **Continue**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/parameter-group-delete.png)

## Associating a Parameter Group with a Cluster<a name="parameter-group-associate"></a>

When you launch a cluster, you must associate it with a parameter group\. If you want to change the parameter group later, you can modify the cluster and choose a different parameter group\. For more information, see [Creating a Cluster by Using Launch Cluster](managing-clusters-console.md#create-cluster-task) and [To modify a cluster](managing-clusters-console.md#modify-cluster-task)\. 