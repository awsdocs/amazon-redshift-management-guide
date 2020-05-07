# Configuring workload management<a name="workload-mgmt-config"></a>

In Amazon Redshift, you use workload management \(WLM\) to define the number of query queues that are available, and how queries are routed to those queues for processing\. WLM is part of parameter group configuration\. A cluster uses the WLM configuration that is specified in its associated parameter group\. 

When you create a parameter group, the default WLM configuration contains one queue that can run up to five queries concurrently\. You can add additional queues and configure WLM properties in each of them if you want more control over query processing\. Each queue that you add has the same default WLM configuration until you configure its properties\.

When you add additional queues, the last queue in the configuration is the *default queue*\. Unless a query is routed to another queue based on criteria in the WLM configuration, it is processed by the default queue\. You can specify mode and concurrency level \(query slots\) for the default queue, but you can't specify user groups or query groups for the default queue\.

 As with other parameters, you cannot modify the WLM configuration in the default parameter group\. Clusters associated with the default parameter group always use the default WLM configuration\. To modify the WLM configuration, create a new parameter group and then associate that parameter group with any clusters that require your custom WLM configuration\. 

## WLM dynamic and static properties<a name="wlm-dynamic-and-static-properties"></a>

The WLM configuration properties are either dynamic or static\. You can apply dynamic properties to the database without a cluster reboot, but static properties require a cluster reboot for changes to take effect\. For more information about static and dynamic properties, see [WLM dynamic and static configuration properties](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-wlm-dynamic-properties.html)\.

## Properties for the wlm\_json\_configuration parameter<a name="wlm-json-config-properties"></a>

You can configure WLM by using the Amazon Redshift console, the AWS CLI, the Amazon Redshift API, or one of the AWS SDKs\. WLM configuration uses several properties to define queue behavior, such as memory allocation across queues, the number of queries that can run concurrently in a queue, and so on\.

**Note**  
 The following properties appear with their Amazon Redshift console names, with the corresponding JSON property names in the descriptions\. 

The following table summarizes whether a property is applicable to automatic WLM or manual WLM\.


****  

| WLM property | Automatic WLM | Manual WLM | 
| --- | --- | --- | 
| Auto WLM | Yes | Yes | 
| Enable short query acceleration | Yes | Yes | 
| Maximum run time for short queries | Yes | Yes | 
| Priority | Yes | No | 
| Queue type | Yes | Yes | 
| Queue name | Yes | Yes | 
| Concurrency Scaling mode | Yes | Yes | 
| Concurrency | No | Yes | 
| User groups | Yes | Yes | 
| User group wildcard | Yes | Yes | 
| Query groups | Yes | Yes | 
| Query group wildcard | Yes | Yes | 
| Timeout | No | Deprecated | 
| Memory | No | Yes | 
| Query Monitoring Rules | Yes | Yes | 

The following list describes the WLM properties that you can configure for each queue\. 

**Auto WLM**  
**Auto WLM** set to `true` enables automatic WLM\. Automatic WLM sets the values for **Concurrency on main** and **Memory \(%\)** to `Auto`\. Amazon Redshift manages query concurrency and memory allocation\. The default is `true`\.  
JSON property: `auto_wlm`

**Enable short query acceleration**  
Short query acceleration \(SQA\) prioritizes selected short\-running queries ahead of longer\-running queries\. SQA executes short\-running queries in a dedicated space, so that SQA queries aren't forced to wait in queues behind longer queries\. With SQA, short\-running queries begin executing more quickly and users see results sooner\. When you enable SQA, you can also specify the maximum run time for short queries\. To enable SQA, specify `true`\. The default is `false`\.  
JSON property: `short_query_queue`

****Maximum run time for short queries****  
When you enable SQA, you can specify 0 to let WLM dynamically set the maximum run time for short queries\. Alternatively, you can specify a value of 1–20 seconds, in milliseconds\. The default value is `0`\.  
JSON property: `max_execution_time`

**Priority**  
Priority sets the priority of queries that run in a queue\. To set the priority, **WLM mode** must be set to **Auto WLM**; that is, `auto_wlm` must be `true`\. Priority values can be `highest`, `high`, `normal`, `low`, and `lowest`\.  The default is `normal`\.  
JSON property: `priority`

**Queue type**  
Queue type designates a queue as used either by **Auto WLM** or **Manual WLM**\. Set `queue_type` to either `auto` or `manual`\. If not specified, the default is `manual`\.  
JSON property: `queue_type`

**Queue name**  
The name of the queue\. You can set the name of the queue based on your business needs\. Queue names must be unique within an WLM configuration, are up to 64 alphanumeric characters, underscores or spaces, and can't contain quotation marks\. For example, if you have a queue for your ETL queries, you might name it `ETL queue`\. This name is used in metrics, system table values, and the Amazon Redshift console to identify the queue\. Queries and reports that use the name from these sources need to be able to handle changes of the name\.  Previously, the queue names were generated by Amazon Redshift\. The default names of queues are `Queue 1`, `Queue 2`, to the last queue named `Default queue`\.   
If you change a queue name, the `QueueName` dimension value of WLM queue metrics \(such as, WLMQueueLength, WLMQueueWaitTime, WLMQueriesCompletedPerSecond, WLMQueryDuration, WLMRunningQueries, and so on\) also changes\. So, if you change the name of a queue, you might need to change CloudWatch alarms you have set up\. 
JSON property: `name`

**Concurrency Scaling mode**  
To enable concurrency scaling on a queue, set **Concurrency Scaling mode** to `auto`\. When the number of queries routed to a queue exceeds the queue's configured concurrency, eligible queries go to the scaling cluster\. When slots become available, queries run on the main cluster\. The default is `off`\.  
JSON property: `concurrency_scaling`

**Concurrency**  
The number of queries that can run concurrently in a manual WLM queue\. This property only applies to manual WLM\. If concurrency scaling is enabled, eligible queries go to a scaling cluster when a queue reaches the concurrency level \(query slots\)\. If concurrency scaling isn't enabled, queries wait in the queue until a slot becomes available\. The range is between 1 and 50\.  
JSON property: `query_concurrency`

**User Groups**  
A comma\-separated list of user group names\. When members of the user group run queries in the database, their queries are routed to the queue that is associated with their user group\.  
JSON property: `user_group`

**User Group Wildcard**  
A Boolean value that indicates whether to enable wildcards for user groups\. If this is 0, wildcards are disabled; if this is 1, wildcards are enabled\. When wildcards are enabled, you can use "\*" or "?" to specify multiple user groups when running queries\. For more information, see [Wildcards](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-defining-query-queues.html#wlm-wildcards)\.  
JSON property: `user_group_wild_card`

**Query Groups**  
A comma\-separated list of query groups\. When members of the query group run queries in the database, their queries are routed to the queue that is associated with their query group\.  
JSON property: `query_group`

**Query Group Wildcard**  
A Boolean value that indicates whether to enable wildcards for query groups\. If this is 0, wildcards are disabled; if this is 1, wildcards are enabled\. When wildcards are enabled, you can use "\*" or "?" to specify multiple query groups when running queries\. For more information, see [Wildcards](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-defining-query-queues.html#wlm-wildcards)\.  
JSON property: `query_group_wild_card`

**Timeout \(ms\)**  
WLM timeout \(`max_execution_time`\) is deprecated\. It is not available when using automatic WLM\. Instead, create a query monitoring rule \(QMR\) using `query_execution_time` to limit the elapsed execution time for a query\. For more information, see [WLM query monitoring rules](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-wlm-query-monitoring-rules.html)\.   
The maximum time, in milliseconds, that queries can run before being canceled\. In some cases, a read\-only query, such as a SELECT statement, might be canceled due to a WLM timeout\. In these cases, WLM attempts to route the query to the next matching queue based on the WLM queue assignment rules\. If the query doesn't match any other queue definition, the query is canceled; it isn't assigned to the default queue\. For more information, see [WLM query queue hopping](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-defining-query-queues.html#wlm-queue-hopping)\. WLM timeout doesn't apply to a query that has reached the `returning` state\. To view the state of a query, see the [STV\_WLM\_QUERY\_STATE](https://docs.aws.amazon.com/redshift/latest/dg/r_STV_WLM_QUERY_STATE.html) system table\.  
JSON property: `max_execution_time`

**Memory \(%\)**  
The percentage of memory to allocate to the queue\. If you specify a memory percentage for at least one of the queues, you must specify a percentage for all other queues, up to a total of 100 percent\. If your memory allocation is below 100 percent across all of the queues, the unallocated memory is managed by the service\. The service can temporarily give this unallocated memory to a queue that requests additional memory for processing\.  
JSON property: `memory_percent_to_use`

**Query Monitoring Rules**  
You can use WLM query monitoring rules to continuously monitor your WLM queues for queries based on criteria, or predicates, that you specify\. For example, you might monitor queries that tend to consume excessive system resources, and then initiate a specified action when a query exceeds your specified performance boundaries\.   
If you choose to create rules programmatically, we strongly recommend using the console to generate the JSON that you include in the parameter group definition\.
You associate a query monitoring rule with a specific query queue\. You can have up to 25 rules per queue, and the total limit for all queues is 25 rules\.  
JSON property: `rules`  
JSON properties hierarchy:   

```
rules
    rule_name
    predicate
        metric_name
        operator
        value
    action
        value
```
For each rule, you specify the following properties:  
+ `rule_name` – Rule names must be unique within WLM configuration\. Rule names can be up to 32 alphanumeric characters or underscores, and can't contain spaces or quotation marks\. You can have up to eight rules per queue, and the total limit for all queues is eight rules\.
  + `predicate` – You can have up to three predicates per rule\. For each predicate, specify the following properties\.
    + `metric_name` – For a list of metrics, see [Query monitoring metrics](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-wlm-query-monitoring-rules.html#cm-c-wlm-query-monitoring-metrics) in the *Amazon Redshift Database Developer Guide*\.
    + `operator` – Operations are `=`, `<`, and `>`\. 
    + `value` – The threshold value for the specified metric that triggers an action\. 
+ `action` – Each rule is associated with one action\. Valid actions are:
  + `log`
  + `hop` \(only available with manual WLM\)
  + `abort`
  + `change_query_priority` \(only available with automatic WLM\)
The following example shows the JSON for a WLM query monitoring rule named `rule_1`, with two predicates and the action `hop`\.   

```
"rules": [
          {
            "rule_name": "rule_1",
            "predicate": [
              {
                "metric_name": "query_execution_time",
                "operator": ">",
                "value": 100000
              },
              {
                "metric_name": "query_blocks_read",
                "operator": ">",
                "value": 1000
              }
            ],
            "action": "hop"
          }
        ]
```

For more information about each of these properties and strategies for configuring query queues, see  [Implementing workload management](https://docs.aws.amazon.com/redshift/latest/dg/cm-c-implementing-workload-management.html) in the *Amazon Redshift Database Developer Guide*\. 

## Configuring the wlm\_json\_configuration parameter using the AWS CLI<a name="Configuring-the-wlm-json-configuration-Parameter"></a>

 To configure WLM, you modify the `wlm_json_configuration` parameter\. The value is formatted in JavaScript Object Notation \(JSON\)\. If you configure WLM by using the AWS CLI, Amazon Redshift API, or one of the AWS SDKs, use the rest of this section to learn how to construct the JSON structure for the `wlm_json_configuration` parameter\. 

**Note**  
 If you configure WLM by using the Amazon Redshift console, you don't need to understand JSON formatting because the console provides an easy way to add queues and configure their properties\. For more information about configuring WLM by using the console, see [Modifying a parameter group](managing-parameter-groups-console.md#parameter-group-modify)\. 

Example

The following example is the default WLM configuration, which defines one queue with automatic WLM\. 

```
{
   "auto_wlm": true
}
```

Example

The following example is a custom WLM configuration, which defines one manual WLM queue with a concurrency level \(query slots\) of five\. 

```
{
   "query_concurrency":5
}
```

Syntax

The default WLM configuration is very simple, with only queue and one property\. You can add more queues and configure multiple properties for each queue in the JSON structure\. The following syntax represents the JSON structure that you use to configure multiple queues with multiple properties: 

```
[
   {
      "ParameterName":"wlm_json_configuration", "ParameterValue":
         "[
             {
                "q1_first_property_name":"q1_first_property_value",
                "q1_second_property_name":"q1_second_property_value", 
                ...
             },
             {
                "q2_first_property_name":"q2_first_property_value",
                "q2_second_property_name":"q2_second_property_value", 
                ...
             }
             ...

         ]"
   }
]
```

In the preceding example, the representative properties that begin with **q1** are objects in an array for the first queue\. Each of these objects is a name/value pair; `name` and `value` together set the WLM properties for the first queue\. The representative properties that begin with **q2** are objects in an array for the second queue\. If you require more queues, you add another array for each additional queue and set the properties for each object\.

 When you modify the WLM configuration, you must include in the entire structure for your queues, even if you only want to change one property within a queue\. This is because the entire JSON structure is passed in as a string as the value for the `wlm_json_configuration` parameter\. 

### Formatting the AWS CLI command<a name="construct-json-param-value"></a>

The `wlm_json_configuration` parameter requires a specific format when you use the AWS CLI\. The format that you use depends on your client operating system\. Operating systems have different ways to enclose the JSON structure so it's passed correctly from the command line\. For details on how to construct the appropriate command in the Linux, Mac OS X, and Windows operating systems, see the sections following\. For more information about the differences in enclosing JSON data structures in the AWS CLI in general, see [Quoting strings](https://docs.aws.amazon.com/cli/latest/userguide/cli-using-param.html#quoting-strings) in the *AWS Command Line Interface User Guide*\.

Examples

The following example command configures manual WLM for a parameter group called `example-parameter-group`\. The configuration enables short\-query acceleration with a maximum run time for short queries set to 0, which instructs WLM to set the value dynamically\. The `ApplyType` setting is `dynamic`\. This setting means that any changes made to dynamic properties in the parameter are applied immediately unless other static changes have been made to the configuration\. The configuration defines three queues with the following: 
+  The first queue enables users to specify `report` as a label \(as specified in the `query_group` property\) in their queries to help in routing queries to that queue\. Wildcard searches are enabled for the `report*` label, so the label doesn't need to be exact for queries to be routed to the queue\. For example, `reports` and `reporting` both match this query group\. The queue is allocated 25 percent of the total memory across all queues, and can run up to four queries at the same time\. Queries are limited to a maximum time of 20000 milliseconds \(ms\)\. mode is set to auto, so when the queue's query slots are full eligible queries are sent to a scaling cluster\. 
+  The second queue enables users who are members of `admin` or `dba` groups in the database to have their queries routed to the queue for processing\. Wildcard searches are disabled for user groups, so users must be matched exactly to groups in the database in order for their queries to be routed to the queue\. The queue is allocated 40 percent of the total memory across all queues, and it can run up to five queries at the same time\. mode is set to off, so all queries sent by members of the admin or dba groups run on the main cluster\.
+  The last queue in the configuration is the default queue\. This queue is allocated 35 percent of the total memory across all queues, and it can process up to five queries at a time\. mode is set to auto\. 

**Note**  
 The example is shown on several lines for demonstration purposes\. Actual commands should not have line breaks\. 

```
aws redshift modify-cluster-parameter-group 
--parameter-group-name example-parameter-group 
--parameters
'[
  {
    "query_concurrency": 4,
    "max_execution_time": 20000,
    "memory_percent_to_use": 25,
    "query_group": ["report"],
    "query_group_wild_card": 1,
    "user_group": [],
    "user_group_wild_card": 0,
    "concurrency_scaling": "auto", 
    "queue_type": "manual"
  },
  {
    "query_concurrency": 5,
    "memory_percent_to_use": 40,
    "query_group": [],
    "query_group_wild_card": 0,
    "user_group": [
      "admin",
      "dba"
    ],
    "user_group_wild_card": 0,
    "concurrency_scaling": "off",
    "queue_type": "manual"    
  },
  {
    "query_concurrency": 5,
    "query_group": [],
    "query_group_wild_card": 0,
    "user_group": [],
    "user_group_wild_card": 0,
    "concurrency_scaling": "auto",
    "queue_type": "manual"    
  },
  {"short_query_queue": true}
]'
```

The following is an example of configuring WLM query monitoring rules for an automatic WLM configuration\. The example creates a parameter group named `example-monitoring-rules`\. The configuration defines the same three queues as the previous example, but the `query_concurrency` and `memory_percent_to_use` are not specified anymore\. The configuration also adds the following rules and query priorities:
+ The first queue defines a rule named `rule_1`\. The rule has two predicates: `query_cpu_time > 10000000` and `query_blocks_read > 1000`\. The rule action is `log`\. The priority of this queue is `Normal`\. 
+ The second queue defines a rule named `rule_2`\. The rule has two predicates: `query_execution_time > 600000000` and `scan_row_count > 1000000000`\. The rule action is `abort`\. The priority of this queue is `Highest`\. 
+ The last queue in the configuration is the default queue\. The priority of this queue is `Low`\. 

**Note**  
 The example is shown on several lines for demonstration purposes\. Actual commands should not have line breaks\. 

```
aws redshift modify-cluster-parameter-group 
--parameter-group-name example-monitoring-rules 
--parameters
'[ {
  "query_group" : [ "report" ],
  "query_group_wild_card" : 1,
  "user_group" : [ ],
  "user_group_wild_card" : 0,
  "concurrency_scaling" : "auto",
  "rules" : [{
    "rule_name": "rule_1",
    "predicate": [{
      "metric_name": "query_cpu_time",
      "operator": ">",
      "value": 1000000 },
      { "metric_name": "query_blocks_read",
      "operator": ">",
      "value": 1000
    } ],
    "action" : "log"
  } ],
   "priority": "normal",
   "queue_type": "auto"
}, {  
  "query_group" : [ ],
  "query_group_wild_card" : 0,
  "user_group" : [ "admin", "dba" ],
  "user_group_wild_card" : 0,
  "concurrency_scaling" : "off",
  "rules" : [ {
    "rule_name": "rule_2",
    "predicate": [
      {"metric_name": "query_execution_time",
      "operator": ">",
      "value": 600000000},
      {"metric_name": "scan_row_count",
      "operator": ">",
      "value": 1000000000}],
      "action": "abort"}],
   "priority": "high",
   "queue_type": "auto"

}, {
  "query_group" : [ ],
  "query_group_wild_card" : 0,
  "user_group" : [ ],
  "user_group_wild_card" : 0,
  "concurrency_scaling" : "auto",
  "priority": "low",
  "queue_type": "auto",
  "auto_wlm": true
}, {
  "short_query_queue" : true
} ]'
```

#### Configuring WLM by using the AWS CLI in the command line with a JSON file<a name="wlm-cli-json-file"></a>

You can modify the `wlm_json_configuration` parameter using the AWS CLI and pass in the value of the `parameters` argument as a JSON file\.

```
aws redshift modify-cluster-parameter-group --parameter-group-name myclusterparaametergroup --parameters file://modify_pg.json 
```

The arguments for `--parameters` are stored in file `modify_pg.json`\. The file location is specified in the format for your operating system\. For more information, see [Loading parameters from a file](https://docs.aws.amazon.com/cli/latest/userguide/cli-usage-parameters.html#cli-usage-parameters-file)\. The following shows examples of the content of the `modify_pg.json` JSON file\.

```
[
    {
        "ParameterName": "wlm_json_configuration",
        "ParameterValue": "[{\"user_group\":\"example_user_group1\",\"query_group\": \"example_query_group1\", \"query_concurrency\":7},{\"query_concurrency\":5}]"
    }
]
```

```
[
    {
        "ParameterName": "wlm_json_configuration",
        "ParameterValue": "[{\"query_group\":[\"reports\"],\"query_group_wild_card\":0,\"query_concurrency\":4,\"max_execution_time\":20000,\"memory_percent_to_use\":25},{\"user_group\":[\"admin\",\"dba\"],\"user_group_wild_card\":1,\"query_concurrency\":5,\"memory_percent_to_use\":40},{\"query_concurrency\":5,\"memory_percent_to_use\":35},{\"short_query_queue\": true, \"max_execution_time\": 5000 }]",
        "ApplyType": "dynamic"
    }
]
```

#### Rules for configuring WLM by using the AWS CLI in the command line on the Linux and macOS X operating systems<a name="wlm-cli-linux-and-mac"></a>

Follow these rules to run an AWS CLI command with parameters on one line:
+ The entire JSON structure must be enclosed in single quotation marks \('\) and brackets \(\[ \]\)\.
+ All parameter names and parameter values must be enclosed in double quotation marks \("\)\.
+ Within the `ParameterValue` value, you must enclose the entire nested structure in double\-quotation marks \("\) and brackets \(\[ \]\)\.
+ Within the nested structure, each of the properties and values for each queue must be enclosed in curly braces \(\{ \}\)\.
+ Within the nested structure, you must use the backslash \(\\\) escape character before each double\-quotation mark \("\)\.
+ For name/value pairs, a colon \(:\) separates each property from its value\.
+ Each name/value pair is separated from another by a comma \(,\)\.
+ Multiple queues are separated by a comma \(,\) between the end of one queue's curly brace \(\}\) and the beginning of the next queue's curly brace \(\{\)\.

#### Rules for configuring WLM by using the AWS CLI in Windows PowerShell on Microsoft Windows operating systems<a name="wlm-cli-windows-powershell"></a>

Follow these rules to run an AWS CLI command with parameters on one line:
+ The entire JSON structure must be enclosed in single quotation marks \('\) and brackets \(\[ \]\)\.
+ All parameter names and parameter values must be enclosed in double quotation marks \("\)\.
+ Within the `ParameterValue` value, you must enclose the entire nested structure in double\-quotation marks \("\) and brackets \(\[ \]\)\.
+ Within the nested structure, each of the properties and values for each queue must be enclosed in curly braces \(\{ \}\)\.
+ Within the nested structure, you must use the backslash \(\\\) escape character before each double\-quotation mark \("\) and its backslash \(\\\) escape character\. This requirement means that you will use three backslashes and a double quotation mark to make sure that the properties are passed in correctly \(\\\\\\"\)\.
+ For name/value pairs, a colon \(:\) separates each property from its value\.
+ Each name/value pair is separated from another by a comma \(,\)\.
+ Multiple queues are separated by a comma \(,\) between the end of one queue's curly brace \(\}\) and the beginning of the next queue's curly brace \(\{\)\.

#### Rules for configuring WLM by using the command prompt on Windows operating systems<a name="wlm-cli-cmd-windows"></a>

Follow these rules to run an AWS CLI command with parameters on one line:
+ The entire JSON structure must be enclosed in double\-quotation marks \("\) and brackets \(\[ \]\)\.
+ All parameter names and parameter values must be enclosed in double quotation marks \("\)\.
+ Within the `ParameterValue` value, you must enclose the entire nested structure in double\-quotation marks \("\) and brackets \(\[ \]\)\.
+ Within the nested structure, each of the properties and values for each queue must be enclosed in curly braces \(\{ \}\)\.
+ Within the nested structure, you must use the backslash \(\\\) escape character before each double\-quotation mark \("\) and its backslash \(\\\) escape character\. This requirement means that you will use three backslashes and a double quotation mark to make sure that the properties are passed in correctly \(\\\\\\"\)\.
+ For name/value pairs, a colon \(:\) separates each property from its value\.
+ Each name/value pair is separated from another by a comma \(,\)\.
+ Multiple queues are separated by a comma \(,\) between the end of one queue's curly brace \(\}\) and the beginning of the next queue's curly brace \(\{\)\.