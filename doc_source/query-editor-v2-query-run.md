# Authoring and running queries<a name="query-editor-v2-query-run"></a>

You can enter a query in the editor or select a saved query from the **Queries** list and choose **Run**\.

By default, **Limit 100** is set to limit the results to 100 rows\. You can turn off this option to return a larger result set\. If you turn off this option, you can include the LIMIT option in your SQL statement if you want to avoid very large result sets\. For more information, see [ORDER BY clause](https://docs.aws.amazon.com/redshift/latest/dg/r_ORDER_BY_clause.html) in the *Amazon Redshift Database Developer Guide*\.

To display a query plan in the results area, turn on **Explain**\. Turn on **Explain graph** for the results to also display a graphical representation of the explain plan\.

To save a query to the **Queries** folder, choose **Save**\.

For a successful query, a success message appears\. If the query returns information, the results display in the **Results** section\. If the number of results exceeds the display area, numbers appear at the top of the results area\. You can choose the numbers to display successive pages of results\.

You can filter and sort **Result** for each column\. To enter filter criteria in the result column header, hover over the column to see a menu \(![\[Filter menu\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/menu.png)\) where you can enter criteria to filter the column\.

If the query contains an error, the query editor v2 displays an error message in the results area\. The message provides information on how to correct the query\.

You can export or copy the results of your query by using the context \(right\-click\) menu in the results area as follows:
+ Choose **Export result set** and either **JSON** or **CSV** to download the entire set of row results to a file\. The number of rows in the result set might be limited by the **Limit** option or the SQL `limit` clause in the query\. The maximum size of the downloaded result set is 5 MB\.
+ If no rows are selected, then choose **Export current page** and either **JSON** or **CSV** to download the rows from the current page to a file\.
+ If rows are selected, then choose **Export selected rows** and either **JSON** or **CSV** to download the rows that are selected to a file\.
+ If rows are selected, then choose **Copy rows** to copy the selected rows to the clipboard\.
+ If rows are selected, then choose **Copy rows with headers** to copy the selected rows with column headers to the clipboard\.

You can also use the shortcut Ctrl\+C on Windows or Cmd\+C on macOS to copy data from the current results page to the clipboard\. If no rows are selected, then the cell with focus is copied to the clipboard\. If rows are selected, then the selected rows are copied to the clipboard\.

To add a new query tab, choose the ![\[New query tab\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) icon, then **Editor**, which appears in the row with the query tabs\. The query tab is either using an `Isolated session` or not\. With an isolated session, the results of a SQL command, such as creating a temporary table in one editor tab, are not visible in another editor tab\. When you open an editor tab in query editor v2, the default is an isolated session\. 

**To run a query**

1. In the query area, do one of the following:
   + Enter a query\.
   + Paste a query that you copied\.
   + Choose the **Queries** folder, open the context menu \(right\-click\) a saved query, and choose **Open query**\.

1. Confirm that you chose the correct **Cluster** or **Workgroup**, and **Database** value for the SQL you plan to run\. 

   Initially, you can choose your **Cluster** or **Workgroup** in the tree view\. Choose your **Database** in the tree view also\.

   You can change the **Cluster** or **Workgroup**, and **Database** within each editor tab with the drop\-down control located near the **Isolated session** header of each editor tab\.

   For each editor tab, you choose whether to run the SQL in an **Isolated session**\. An isolated session has its own connection to a database\. Use it to run SQL that is isolated from other query editor sessions\. For more information about connections, see [Opening query editor v2](query-editor-v2-using.md#query-editor-v2-open)\.

1. Choose **Run**\.

   The **Result** area opens and displays the query results\.

**To display the explain plan for a query**

1. Select the query\.

1. Turn on **Explain**\.

   By default, the **Explain graph** is also on\.

1. Choose **Run**\.

   The query runs and the explain plan displays in the query **Result** area\.

The query editor v2 supports the following features:
+ You can author queries with multiple SQL statements in one query tab\. The queries are run serially and multiple results tabs open for each query\. 
+ You can author queries with session variables and temporary tables\.
+ You can author queries with replaceable parameters designated by `${parameter}`\. You can author your SQL query with multiple replaceable parameters and use the same parameter in multiple places in your SQL statement\. 

  When the query runs, a window is presented to enter the value of the parameter\. Each time you run the query, the window is presented to enter your parameter values\. 

  For an example, see [Example: Sales greater than a specific parameter](#query-editor-v2-example-sales-qtysold-greater-than-parameter)\. 
+ Queries are versioned automatically\. You can choose an earlier version of a query to run\.
+ You don't need to wait for a query to complete before continuing on with your workflow\. Your queries continue to run even if you close the query editor\.
+ When authoring queries, automatic completion of schema, table, and column names is supported\.

The SQL editor supports the following features:
+ The beginning and ending brackets used in SQL have matching colors\. Vertical lines are shown in the editor to help you match brackets\.
+ You can collapse and expand sections of your SQL\. 
+ You can search and replace text in your SQL\.
+ You can use shortcut keys for several common editing tasks\.
+ SQL errors are highlighted in the editor for convenient location of problem areas\.

For a demo of editor features, watch the following video\. 

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/9JAq0yDs0YE/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/9JAq0yDs0YE)

## Query examples<a name="query-editor-v2-examples"></a>

Following, you can find descriptions of the various types of queries that you can run\. 

The data used in many of these queries is from the `tickit` sample schema\. For more information about the `tickit` data, see [Loading sample data](query-editor-v2-loading.md#query-editor-v2-loading-sample-data)\.

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
+ You can run a query up to 300,000 characters long\. 
+ You can save a query up to 30,000 characters long\. 
+ You can't use transactions in the editor\. For more information about transactions, see [BEGIN](https://docs.aws.amazon.com/redshift/latest/dg/r_BEGIN.html) in the *Amazon Redshift Database Developer Guide\.*