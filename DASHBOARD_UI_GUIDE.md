# Creating the Ground Station Dashboard via the QuickSight UI

This guide walks through manually creating the Ground Station contacts dashboard in Amazon QuickSight. Use this if you set the `DeployDashboard` parameter to `no` when deploying the `gsdashboard-part2.yml` CloudFormation stack. In that case, the DataSource and DataSet are still created automatically — only the dashboard itself is skipped.

> QuickSight console labels and layouts change over time. For the mechanics of adding visuals, filters, and dashboards, refer to the [Amazon Quick Sight User Guide](https://docs.aws.amazon.com/quicksuite/latest/userguide/welcome.html). If this guide and `cfn/gsdashboard-part2.yml` disagree, the CloudFormation template is the source of truth.

## Prerequisites

- Stack 1 and Stack 2 (without the GSDashboard resource) have been deployed successfully
- The following resources exist in QuickSight:
  - DataSource: **GSDashboardQSDataSource**
  - DataSet: **GSDashboardQSDataset**
- You have QuickSight author or admin permissions

## Step 1: Create a new analysis

Create a new **Interactive sheet** analysis on the **GSDashboardQSDataset** dataset. See [Starting an analysis](https://docs.aws.amazon.com/quicksuite/latest/userguide/creating-an-analysis.html).

## Step 2: Rename the sheet

Rename the default sheet to **Contacts Overview**.

## Step 3: Add the contacts table

Add a **Table** visual with the following fields in **Group by** (in this order):

- `contactid`
- `groundstation`
- `contactstatus`
- `satellitearn`
- `missionprofilearn`
- `starttime`
- `endtime`
- `curcontactid`
- `tags`
- `region`

Set the aggregation for `starttime` and `endtime` to **Second**.

Title: **AWS Ground Station contacts information and mapping to Cost and Usage Report fields**

Size the visual so more rows and columns are visible. See [Adding a visual](https://docs.aws.amazon.com/quicksuite/latest/userguide/creating-a-visual.html) and [Formatting data labels](https://docs.aws.amazon.com/quicksuite/latest/userguide/customizing-visual-data-labels.html).

## Step 4: Add the "Contacts by Status" pie chart

Add a **Pie chart** visual with:

- **Group/Color**: `contactstatus`
- **Value**: `contactid` (Count)
- **Data labels**: on, with metric shown

Title: **Count of contacts by contact status**

## Step 5: Add the "Contacts by Ground Station" pie chart

Add a **Pie chart** visual with:

- **Group/Color**: `groundstation`
- **Value**: `contactid` (Count)
- **Data labels**: on, with metric shown

Title: **Count of contacts by Ground Station**

## Step 6: Arrange the layout

Drag and resize the visuals so they match this layout:

```
+--------------------------------------------------+
|                                                  |
|              Contacts Table (full width)         |
|                                                  |
+------------------------+-------------------------+
|                        |                         |
|  Status Pie Chart      |  Ground Station Pie     |
|  (left half)           |  Chart (right half)     |
|                        |                         |
+------------------------+-------------------------+
```

- The table should span the full width of the sheet
- The two pie charts should be side by side below the table, each taking half the width

## Step 7: Add filters

Add a filter on each of the following fields, scoped to the contacts table. For each filter, add a filter control and pin it to the top of the sheet.

- `contactid`
- `groundstation`
- `contactstatus`
- `satellitearn`
- `missionprofilearn`
- `starttime`
- `endtime`
- `curcontactid`
- `tags`
- `region`

See [Filtering data](https://docs.aws.amazon.com/quicksuite/latest/userguide/adding-a-filter.html) and [Filter controls](https://docs.aws.amazon.com/quicksuite/latest/userguide/filter-controls.html).

## Step 8: Publish as a dashboard

Publish the analysis as a new dashboard named **GSDashboard**. See [Creating a dashboard](https://docs.aws.amazon.com/quicksuite/latest/userguide/creating-a-dashboard.html).
