# Creating the Ground Station Dashboard via the QuickSight UI

This guide walks through manually creating the Ground Station contacts dashboard in Amazon QuickSight. Use this if you set the `DeployDashboard` parameter to `no` when deploying the `gsdashboard-part2.yml` CloudFormation stack. In that case, the DataSource and DataSet are still created automatically — only the dashboard itself is skipped.

## Prerequisites

- Stack 1 and Stack 2 (without the GSDashboard resource) have been deployed successfully
- The following resources exist in QuickSight:
  - DataSource: **GSDashboardQSDataSource**
  - DataSet: **GSDashboardQSDataset**
- You have QuickSight author or admin permissions

## Step 1: Create a new analysis

1. Open the QuickSight console
2. Click **Analyses** in the left navigation
3. Click **New analysis**
4. Select the **GSDashboardQSDataset** dataset
5. Click **Use in analysis**
6. Select **Interactive sheet** when prompted

## Step 2: Rename the sheet

1. Double-click the sheet tab at the top (default name is "Sheet 1")
2. Rename it to **Contacts Overview**

## Step 3: Add the contacts table

1. Click **Add** → **Add visual**
2. Select **Table** as the visual type
3. In the **Field wells** panel, drag the following fields into **Group by** (in this order):
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
4. Click the title area and set it to: **AWS Ground Station contacts information and mapping to Cost and Usage Report fields**

### Configure the date fields

5. Click the `starttime` field in the field wells → change aggregation to **Second**
6. Click the `endtime` field in the field wells → change aggregation to **Second**

### Resize the table

7. Drag the edges of the table visual to make it taller and wider so more rows and columns are visible
8. Column widths can be adjusted by dragging the column borders in the table header if needed

### Configure table styling

9. Hover over the table visual and click the pencil icon ("Format visual") in the top right corner of the visual to adjust properties like header style, cell height, text wrapping, etc.

## Step 4: Add the "Contacts by Status" pie chart

1. Click **Add** → **Add visual**
2. Select **Pie chart** as the visual type
3. In the field wells:
   - **Group/Color**: drag `contactstatus`
   - **Value**: drag `contactid` (set aggregation to Count)
4. Click the title area and set it to: **Count of contacts by contact status**
5. Hover over the pie chart visual and click the pencil icon in the top right corner of the visual → configure:
   - **Data labels**: turn on, enable **Show metric**

## Step 5: Add the "Contacts by Ground Station" pie chart

1. Click **Add** → **Add visual**
2. Select **Pie chart** as the visual type
3. In the field wells:
   - **Group/Color**: drag `groundstation`
   - **Value**: drag `contactid` (set aggregation to Count)
4. Click the title area and set it to: **Count of contacts by Ground Station**
5. Hover over the pie chart visual and click the pencil icon in the top right corner of the visual → configure:
   - **Data labels**: turn on, enable **Show metric**

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

First, click on the **Contacts Table** visual to select it. Then add the following filters by clicking the **Filter** icon (funnel icon) in the top panel → **Add filter**:

| Filter | Field |
|--------|-------|
| contactid | `contactid` |
| groundstation | `groundstation` |
| contactstatus | `contactstatus` |
| satellitearn | `satellitearn` |
| missionprofilearn | `missionprofilearn` |
| starttime | `starttime` |
| endtime | `endtime` |
| curcontactid | `curcontactid` |
| tags | `tags` |
| region | `region` |

For each filter:
1. Click **Add filter** → select the field
2. Click the three dots on the filter → **Manage control** → **Move to top of sheet**

## Step 8: Publish as a dashboard

1. Click **Share** in the top right
2. Click **Publish dashboard**
3. Name it **GSDashboard**
4. Click **Publish dashboard**
