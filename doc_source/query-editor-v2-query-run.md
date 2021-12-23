# Authoring and running queries<a name="query-editor-v2-query-run"></a>

You can enter a query in the editor or select a saved query from the **Queries** list and choose **Run**\.

By default, **Limit 100** is set to limit the results to 100 rows\. You can turn off this option to return a larger result set\. If you turn off this option, you can include the LIMIT option in your SQL statement if you want to avoid very large result sets\. For more information, see [ORDER BY clause](https://docs.aws.amazon.com/redshift/latest/dg/r_ORDER_BY_clause.html) in the *Amazon Redshift Database Developer Guide*\.

To display a query plan in the results area, turn on **Explain**\.

To save a query to the **Queries** folder, choose **Save**\.

For a successful query, a success message appears\. If the query returns information, the results display in the **Results** section\. If the number of results exceeds the display area, numbers appear at the top of the results area\. You can choose the numbers to display successive pages of results\.

You can filter and sort **Result** for each column\. To enter filter criteria in the result column header, hover over the column to see a menu \(![\[Filter menu\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/menu.png)\) where you can enter criteria to filter the column\.

If the query contains an error, the query editor v2 displays an error message in the results area\. The message provides information on how to correct the query\.

You can export the query results on the current page to a file in JSON or comma\-separated value \(CSV\) format\. To save the file in either format, open the context \(right\-click\) menu in the results area, then choose **Export current page** and either **JSON** or **CSV**\. You can also select rows and export the results for specific rows\.

To add a new query tab, choose the ![\[New query tab\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) tab, which appears in the row with the query tabs\. 

**To run a query**

1. In the query area, do one of the following:
   + Enter a query\.
   + Paste a query that you copied\.
   + Choose the **Queries** folder, open the context menu \(right\-click\) a saved query, and choose **Open query**\.

1. Confirm that you chose the correct **Cluster** and **Database** value for the SQL you plan to run\. 

   Change your **Cluster** value by choosing another cluster in the tree view\. Change your database by choosing a different **Database** in the interface\.

1. Choose **Run**\.

   The **Result** area opens and displays the query results\.

**To display the explain plan for a query**

1. Select the query\.

1. Turn on **Explain**\.

   By default, the **Explain graph** is also on\.

1. Choose **Run**\.

   The query runs and the explain plan displays in the query **Result** area\.

The query editor v2 supports the following features:
+ You can author queries with multiple SQL statements in one query tab\. The queries are run in serially and multiple results tabs open for each query\. 
+ You can run queries in parallel by opening a query tab, choose a database, run the query\. While that query is running, you can choose a different database \(and different cluster if you want\), and run another query\.
+ You can author queries with session variables and temporary tables\.
+ You can author queries with replaceable parameters designated by `${parameter}`\. You can author your SQL query with multiple replaceable parameters and use the same parameter in multiple places in your SQL statement\. 

  When the query runs, a window is presented to enter the value of the parameter\. Each time you run the query, the window is presented to enter your parameter values\. 

  For an example, see [Example: Sales greater than a specific parameter](#query-editor-v2-example-sales-qtysold-greater-than-parameter)\. 
+ Queries are versioned automatically\. You can choose an earlier version of a query to run\.
+ You don't need to wait for a query to complete before continuing on with your workflow\. Your queries continue to run even if you close the query editor\.
+ When authoring queries, automatic completion of schema, table, and column names is supported using a shortcut\. 

  Choose the ![\[Shortcuts\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/shortcuts-icon.png) **Shortcuts** icon to find the **Auto complete** shortcut\.

The following examples demonstrate some of these features\.

## Query examples<a name="query-editor-v2-examples"></a>

Following, you can find descriptions of the various types of queries that you can run\. 

The data used in these queries is from the `tickit` sample schema\.  For more information about the `tickit` data, see [Sample database](https://docs.aws.amazon.com/redshift/latest/dg/c_sampledb.html) in the *Amazon Redshift Database Developer Guide*\. 

When you run these example queries, confirm that you choose the correct database in the editor, such as `sample_data_dev`\.

**Topics**
+ [Example: Setting session variables](#query-editor-v2-example-set-session-variable)
+ [Example: Top event by total sales](#query-editor-v2-example-top-event-sales)
+ [Example: Sales greater than a specific parameter](#query-editor-v2-example-sales-qtysold-greater-than-parameter)
+ [Example: Create a temporary table](#query-editor-v2-example-create-temporary-table)
+ [Example: Selecting from a temporary table](#query-editor-v2-example-select-from-temporary-table)

### Example: Setting session variables<a name="query-editor-v2-example-set-session-variable"></a>

The following command sets the `search_path` server configuration parameter to *public* for the session\. For more information, see [SET](https://docs.aws.amazon.com/redshift/latest/dg/r_SET.html) and [search\_path](https://docs.aws.amazon.com/redshift/latest/dg/r_search_path.html) in the *Amazon Redshift Database Developer Guide*\. 

```
set search_path to public;
```

### Example: Top event by total sales<a name="query-editor-v2-example-top-event-sales"></a>

The following query finds the event with the most sales\. 

```
select eventname, count(salesid) totalorders, sum(pricepaid) totalsales
from sales, event
where sales.eventid=event.eventid
group by eventname
order by 3;
```

Following is a partial list of the results\.

```
eventname           totalorders       totalsales
White Christmas         20              9352
Joshua Radin            38             23469
Beach Boys              58             30383
Linda Ronstadt          56             35043
Rascal Flatts           76             38214
Billy Idol              67             40101
Stephenie Meyer         72             41509
Indigo Girls            57             45399
...
```

### Example: Sales greater than a specific parameter<a name="query-editor-v2-example-sales-qtysold-greater-than-parameter"></a>

The following query finds sales where the quantity sold is greater than the parameter specified by `${numberoforders}`\. When the parameter value is `7`, the result is 60 rows\. When you run the query, the query editor v2 displays a **Run query form** window to gather the value of parameters in the SQL statement\. 

```
select salesid, qtysold
from sales 
where qtysold > ${numberoforders}
order by 2;
```

Following is a partial list of the results\.

```
salesid	qtysold
20005	8
21279	8
130232	8
42737	8
74681	8
67103	8
105533	8
91620	8
121552	8
...
```

### Example: Create a temporary table<a name="query-editor-v2-example-create-temporary-table"></a>

The following statement creates the temporary table *eventsalestemp* by selecting information from the *sales* and *event* tables\. 

```
create temporary table eventsalestemp as
select eventname, count(salesid) totalorders, sum(pricepaid) totalsales
from sales, event
where sales.eventid=event.eventid
group by eventname;
```

### Example: Selecting from a temporary table<a name="query-editor-v2-example-select-from-temporary-table"></a>

The following statement selects events, total orders, and total sales from the temporary table *eventsalestemp*, ordered by total orders\. 

```
select eventname,  totalorders,  totalsales
from eventsalestemp
order by 2;
```

Following is a partial list of results\.

```
eventname          totalorders   totalsales
White Christmas        20          9352
Joshua Radin           38         23469
Martina McBride        50         52932
Linda Ronstadt         56         35043
Indigo Girls           57         45399
Beach Boys             58         30383
...
```

## Considerations for querying<a name="query-editor-v2-considerations"></a>

Consider the following information about working with queries when you use the query editor:
+ The maximum query result size is the smaller of 5 MB or 100,000 rows\. 
+ You can export only the current page of results or selected result rows to a JSON or CSV file\. 
+ You can run a query up to 300,000 characters long\. 
+ You can save a query up to 30,000 characters long\. 
+ You can't use transactions in the editor\. For more information about transactions, see [BEGIN](https://docs.aws.amazon.com/redshift/latest/dg/r_BEGIN.html) in the *Amazon Redshift Database Developer Guide\.*