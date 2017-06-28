## KyAnalyzer Self-service Agile BI Tools

KyAnalyzer allow user analyze data easier and quicker.


###Pre-Condition
* KAP should be version 2.1 or later
* Apache Kylin version should be 1.5.4.1 or later.
* KyAnalyzer does not supprt *left join*, you should use *inner join* when create cube mode in KAP.

### Install
Get the package KyAnalyzer-{version}.tar.gz from Kyligence Inc, and download the customized mondrian mondrian-kylin-{version}.jar from GitHub kylin-mondrian repository: (https://github.com/Kyligence/kylin-mondrian/blob/master/build/), if the version is updated, please use the latest jar.
Uncompress KyAnalyzer package, copy mondrian-kylin-{version}.jar into server/tomcat/webapps/saiku/WEB-INF/lib.

Edit kyanalyzer.properties under kyanalyzer-server/conf，set KAP host and KAP port, *kap.host* represents KAP IP(default value localhost), and *kap.port* represents KAP app port(default value 7070). And you can edit mondrian.properties refer to conf/mondrian.properties.template.（Note: we have moved 'kap.host' and 'kap.port' to kyanalyzer.properties since kap2.2, and also add mondrian.properties to kyanalyzer-server/conf/）

Run start-analyzer.sh under server to start KyAnalyzer, the default port is 8080. When the server is started, please visit http://{hostname}:8080. If you want to stop the server, please run stop-analyzer.sh.

 If the server is not started normally, please check the logs under tomcat/logs for details. Make sure there's no port conflict, you can check it in tomcat/logs/catalina.out, and update to new port in tomcat/conf/server.xml.


#### Files under home directory

![](images/server_dir.png)

KyAnalyzer will save data under the directory *repository* and *data*, when you upgrade  KyAnalyzer, need to backup these two directories.

###About KyAnalyzer,KAP,Mondrian-Kylin Version/Features
<table>
    <tr>
    <th>KAP</th>
    <th>KyAnalyzer</th>
    <th>Mondrian-Kylin</th>
    <th>COUNT_DISTINCT</th>
    <th>TOP_N</th>
    <th>Normal Query</th>
    <th></th>
    </tr>
    <tr>
        <td>2.0</td>
        <td>2.1</td>
        <td>1.0</td>
        <td>❎</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr>
    <tr>
        <td>2.0</td>
        <td>2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>❎</td>
        <td>❎</td>
        <td></td>
    </tr>
    <tr>
        <td>2.1</td>
        <td>2.1</td>
        <td>1.0</td>
        <td>❎</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr> 
    <tr>
        <td>2.1</td>
        <td>2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>✅</td>
        <td>✅</td>
        <td>Recommended</td>
    </tr>     
</table>


###About KyAnalyzer,Apache Kylin,Mondrian-Kylin Version/Features
<table>
    <tr>
    <th>Apache Kylin</th>
    <th>KyAnalyzer</th>
    <th>Mondrian-Kylin</th>
    <th>COUNT_DISTINCT</th>
    <th>TOP_N</th>
    <th>Normal Query</th>
    <th></th>
    </tr>
    <tr>
        <td>ALL</td>
        <td>2.1</td>
        <td>1.0</td>
        <td>❎</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr>
    <tr>
        <td>version earlier than1.5.4.1</td>
        <td>2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>❎</td>
        <td>❎</td>
        <td></td>
    </tr>
    <tr>
        <td>1.5.4.1</td>
        <td>2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr> 
    <tr>
        <td>version later 1.5.4.1</td>
        <td>2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>✅</td>
        <td>✅</td>
        <td>Recommended</td>
    </tr>         
</table>


### Authentication
KyAnalyzer authenticates user at KAP side, so you can use KAP user and password to login. Users with *Admin* Role in KAP are also *Admin* in KyAnalyzer.
![](images/analyzer_login.png)

### Admin Page
Only the admin could visit this page.
To import the Cube from KAP, KyAnalyzer needs to create schema and datasource for each cube in KAP by default. KyAnalyzer automatize the flow, user don't have to write schema by themeselves.
Click `Sync Cubes From Kylin` on the left side, the right panel will list all projects from KAP.

![](images/admin_sync.png)

Click the green button `Sync Cubes From Kylin` after select project, KyAnalyzer will create *schema* and *datasource* for all cubes under this project whose status is 'READY'.

![](images/sync_done_tip.png)

The schema could be edited online in KyAnalyzer, it's not necessary at most time.

### User's query authority to cubes
User's query authority to cubes in KyAnalyzer is consistent with kylin/kap's, for details, please refer to [Manage Permissions](../security/acl.en.md).

### New Query
Click `New Query` button at navigation bar, then click `Refresh` button to fetch the latest meta data, select the cube under *Cubes* menu, now all dimensions and measures are shown. In the report designer area, here are four panels: *Measures*, *Columns*, *Rows*, *Filter*. User could drag measures into Measures panel, and dimensions into Columns and Rows panels.

![](images/cube_refresh.png)

KyAnalyzer supports many types of visulization, table, bar, stacked bar, line chart, area chart, heat grid, tree map, pie chart, dot chart, water fall chart ,etc.

#### Bar
![](images/bar_chart.png)

#### Pie
![](images/pie_chart.png)

#### Pivotal Table
![](images/pivotal_table.png)

#### COUNT DISTINCT
![](images/count_distinct.png)

#### Hierarchy Data
![](images/hierarchy_table.png)


### Result Filter
To filter the results, you can click the dimension name and see the filter dialog, input the search pattern to filter the select options, and select your options, you can include or exclude the options.

![](images/filter.png)

### Save Query Result
You can export results to files(csv, pdf, excel) or pictures.

#### Export grid to files
![](images/export_table.png)

#### Export chart to picture
![](images/export_image.png)

### Save query results in KyAnalyzer
The query result could be saved in KyAnalyzer, so next time when you login, you don't have to select your dimensions and measures again.

#### Save Query
![](images/save_query.png)

#### Open Query
![](images/open_query.png)

#### Execute Query
![](images/execute_query.png)






