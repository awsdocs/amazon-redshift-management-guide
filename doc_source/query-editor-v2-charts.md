# Visualizing query results<a name="query-editor-v2-charts"></a>

After you run a query and the results display, you can turn on **Chart** to display a graphic visualization of the results\. You can use the following controls to define the content, structure, and appearance of your chart:

![\[Trace\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) Trace  
Represents a set of related graphical marks in a chart\. You can define multiple traces in a chart\.

Type  
You can define the trace type to represent data as one of the following:  
+ Scatter chart for a scatter plot or bubble chart\.
+ Bar chart to represent categories of data with vertical or horizontal bars\.
+ Area chart to define filled areas\.
+ Histogram that uses bars to represent frequency distribution\.
+ Pie chart for a circular representation of data where each slice represents a percentage of the whole\.
+ Funnel or Funnel Area chart to represent data through various stages of a process\.
+ OHLC \(open\-high\-low\-close\) chart often used for financial data to represent open, high, low, and close values along the x\-axis, which usually represents intervals of time\.
+ Candlestick chart to represent a range of values for a category over a timeline\.
+ Waterfall chart to represent how an initial value increases or decreases through a series of intermediate values\. Values can represent time intervals or categories\.
+ Line chart to represent changes in value over time\.

X axis  
You specify a table column that contains values to plot along the X axis\. Columns that contain descriptive values usually represent dimensional data\. Columns that contain quantitative values usually represent factual data\.

Y axis  
You specify a table column that contains values to plot along the Y axis\. Columns that contain descriptive values usually represent dimensional data\. Columns that contain quantitative values usually represent factual data\.

Subplots  
You can define additional presentations of chart data\.

Transforms  
You can define transforms to filter trace data\. You use a split transform to display multiple traces from a single source trace\. You use an aggregate transform to present a trace as an average or minimum\. You use a sort transform to sort a trace\.

General appearance  
You can set defaults for background color, margin color, color scales to design palettes, text style and sizes, title style and size, and mode bar\. You can define interactions for drag, click, and hover\. You can define meta text\. You can define default appearances for traces, axes, legends, and annotations\.

Choose **Traces** to display the results as a chart\. For **Type**, choose the style of chart as **Bar**, **Line**, and so on\. For **Orientation**, you can choose **Vertical** or **Horizontal**\. For **X**, choose the table column that you want to use for the horizontal axis\. For **Y**, choose the table column that you want to use for the vertical axis\.

Choose **Refresh** to update the chart display\. Choose **Full screen** to expand the chart display\.

**To create a chart**

1. Run a query and get results\.

1. Turn on **Charts**\.

1. Choose **Trace** and start to visualize your data\.

1. Choose a chart style from one of the following:
   + Scatter
   + Bar
   + Area
   + Histogram
   + Pie
   + Funnel
   + Funnel Area
   + OHLC \(open\-high\-low\-close\)
   + Candlestick
   + Waterfall
   + Line

1. Choose **Style** to customize the appearance, including colors, axes, legend, and annotations\. You can add text, shapes, and images\.

1. Choose **Annotations** to add text, shapes, and images\.

**To save a chart**

1. Choose **Save Chart**\.

1. Enter a name for your chart\.

1. Choose **Save**\. 

**To export a chart**

1. Choose **Export**\.

1. Choose **PNG** or **JPEG**\. 

1. Set the width and height for your chart\.

1. Choose **Export**\. 

1. Choose to open the file in your default graphic application or save the file with the default name\. 

**To browse for and open a saved chart**

1. Choose the **Charts** tab\.

1. Open the chart that you want\.

**To organize your charts into folders**

1. Choose **Charts** from the navigation pane\.

1. Choose **New folder** and name the folder\. 

1. Choose **Create** to create the folder in the **Charts** tab\. 

   You can move charts in and out of the folder using drag\-and\-drop\.

## Example: Create a pie chart to visualize query results<a name="query-editor-v2-example-pie-chart"></a>

The following example uses the *Sales* table of the sample database\. For more information, see [Sample database](https://docs.aws.amazon.com/redshift/latest/dg/c_sampledb.html) in the *Amazon Redshift Database Developer Guide*\. 

Following is the query that you run to provide the data for the pie chart\.

```
select top 5 eventname, count(salesid) totalorders, sum(pricepaid) totalsales 
from sales, event
where sales.eventid=event.eventid group by eventname
order by 3;
```

**To create a pie chart for the top event by total sales**

1. Run the query\.

1. In the query results area, turn on **Chart**\.

1. Choose **Trace**\.

1. For **Type**, choose **Pie**\.

1. For **Values**, choose *totalsales*\.

1. For **Labels**, choose *eventname*\.

1. Choose **Style** and then **General**\.

1. Under **Colorscales**, choose **Categorical** and then **Pastel2**\.

![\[Pie chart\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/pie-chart.png)

## Example: Create a combination chart for comparing revenue and sales<a name="query-editor-v2-example-revenue-sales-chart"></a>

Perform the steps in this example to create a chart that combines a bar chart for revenue data and a line graph for sales data\. The following example uses the *Sales* table of the tickit sample database\. For more information, see [Sample database](https://docs.aws.amazon.com/redshift/latest/dg/c_sampledb.html) in the *Amazon Redshift Database Developer Guide*\. 

Following is the query that you run to provide the data for the chart\.

```
select eventname, total_price, total_qty_sold
from  (select eventid, total_price, total_qty_sold, ntile(1000) over(order by total_price desc) as percentile
       from (select eventid, sum(pricepaid) total_price, sum(qtysold) total_qty_sold
             from   tickit.sales
             group by eventid)) Q, tickit.event E
       where Q.eventid = E.eventid
       and percentile = 1
order by total_price desc;
```

**To create a combination chart for comparing revenue and sales**

1. Run the query\.

1. In the query results area, turn on **Chart**\.

1. Under *trace o*, for **Type**, choose **Bar**\.

1. For **X**, choose *eventname*\.

1. For **Y**, choose *total\_price*\.

   The bar chart displays with event names along the X axis\.

1. Under **Style**, choose **Traces**\. 

1. For **Name**, enter *Revenue*\.

1. Under **Style**, choose **Axes**\. 

1. For **Titles**, choose **Y** and enter *Revenue*\.

   The label *Revenue* displays on the left Y axis\.

1. Under **Structure**, choose **Traces**\.

1. Choose ![\[Trace\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png) **Trace**\.

   The trace 1 options display\.

1. For **Type**, choose **Line**\.

1. For **X**, choose *eventname*\.

1. For **Y**, choose *total\_qty\_sold*\.

1. Under **Axes To Use**, for **Y Axis** choose ![\[Trace\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/add-plus.png)\. 

   The **Y Axis** displays *Y2*\.

1. Under **Style**, choose **Axes**\.

1. Under **Titles**, choose **Y2**\.

1. For **Name**, enter *Sales*\.

1. Under **Lines**, choose *Y:Sales*\.

1. Under **Axis Line**, choose **Show** and for **Position**, choose **Right**\.

![\[Revenue and sales chart\]](http://docs.aws.amazon.com/redshift/latest/mgmt/images/chart-revenue-sales.png)

## Demo: Build visualizations using Amazon Redshift query editor v2<a name="query-editor-v2-demo-visualizations"></a>

For a demo of how to build visualizations, watch the following video\. 

[![AWS Videos](http://img.youtube.com/vi/https://www.youtube.com/embed/-FYqTIER-6U/0.jpg)](http://www.youtube.com/watch?v=https://www.youtube.com/embed/-FYqTIER-6U)