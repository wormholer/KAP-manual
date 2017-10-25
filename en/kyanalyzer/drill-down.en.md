## Drill-down and Roll-up

In the project of *Learn_kylin*, the cube *kylin_sales_cube* contains the hierarchy as below. We will demonstrate how to perform the actions of drill-down and roll-up by taking the *kylin_sales_cube* as an example.

![Hierarchy](images/hierarchy_en.png)

### Drill-down

#### Drill-down without filter

1. To drill down, first place one dimension of the hierarchy and a measure on a report. 

2. Click on the dimension and choose **Include Level**.
  ![Drill-down Report 1](images/drill_down_on_report-1.png)

3. Choose the dimension that you want to drill down to.

4. The report will return with the dimension that you choose to drill down to. 
  ![Drill-down Report 2](images/drill_down_on_report-2.png)

#### Drill-down with filter

You may also choose to drill down with the filter applied. The report will filter on the value of the dimension you selected.

1. To drill down, first place one dimension of the hierarchy and a measure on a report. 

2. Click on the dimension and choose **Keep and Include Level**.

3. Choose the dimension that you want to drill down to.
  ![Report 1 of Drill-down with Filter](images/drill_down_with_filter_on_report-1.png)

4. The report will return with the dimension that you choose to drill down to. 

In this example, by clicking on the value **Computers/Tablets & Networking** to trigger the filter, the drill-down with the filter will drill down with "Computers/Tablets & Networking" value only.
![Report 2 of Drill-down with Filter](images/drill_down_with_filter_on_report-2.png)

### Roll-up

You may also be able to roll up the dimension that you drilled down.

1. To roll up, first the hierarchy dimension that can be rolled up needs to be on the report.

2. Click on the dimension that you may want to roll up and choose **Remove Level**.

3. Choose the dimension that you want to roll up.

   ![Roll-up Report 1](images/roll_up_on_report-1.png)

4. The report will return with the roll-up dimension removed. 

![Roll-up Report 2](images/roll_up_on_report-2.png)

###Drill-across

In addition to the actions of drill-down and roll-up, you may also be able to drill across to other dimensions that are not in the hierarchy.

#### Drill Across on Row

1. Click on the drill-cross icon on the top menu.

   ![Drill Across on Row 1](images/drill_across_on row_with_filter_using_menu-1.png)

2. Click on the measure.

3. On the pop-up menu, choose the dimension and measure you may want to drill across.

4. The result will return with the dimension and measure you selected in the previous step.

![Drill Across on Row 2](images/drill_across_on row_with_filter_using_menu-2.png)

![Drill Across on Row 3](images/drill_across_on row_with_filter_using_menu-3.png)

#### Drill Across on Column

1. Click on the drill-cross icon on the top menu.
2. Click on the measure.
3. On the pop-up menu, choose the dimension and measure you may want to drill across.
4. The result will return with the dimension and measure you selected in the previous step.

![Drill Across on Column 1](images/drill_across_on_column_with_filter_using_menu-1.png)

![Drill Across on Column 2](images/drill_across_on_column_with_filter_using_menu-2.png)

![Drill Across on Column 3](images/drill_across_on_column_with_filter_using_menu-3.png)
