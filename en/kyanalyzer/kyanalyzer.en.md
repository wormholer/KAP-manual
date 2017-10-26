## KyAnalyzer Self-service Agile BI Tools

KyAnalyzer allows users to analyze their data easier and quicker.

### Pre-Conditions
* KAP version should be  2.1 or later.
* Apache Kylin version should be 1.5.4.1 or later.
* KyAnalyzer does not supprt the query of *left join* currently. You may use *inner join* when creating a cube model in KAP.

### Installation
1. Go to [KyAccount]( http://account.kyligence.io/) to apply for KyAnalyzer's installation package and license. 

2. Unzip KyAnalyzer's install package, it will then generate the folder *kyanalyzer-server-{version}*:

   ```tar-zxf KyAnalyzer-{version}.tar.gz```

In KyAnalyzer V2.4.0 or earlier version, copy the license file of *kyAnalyzer.lic* to *kyanalyzer-{version}/conf*.

   ```mv kyAnalyzer.lic kyanalyzer-{version}/conf```

In KyAnalyzer V2.5.0 or later version, KyAnalyzer will read the KAP license and does not require a separate license file.

3. KyAnalyzer depends on the jar package of mondrian. It needs to be downloaded and copied to KyAnalyzer's path in order to comply with open source license.
   * For KyAnalyzer V2.1.3 and earlier versions, a user is required to download the jar from [mondrian-kylin-1.2.jar]( https://github.com/Kyligence/kylin-mondrian/blob/master/build/mondrian-kylin-1.2.jar), and copy it to the folder *kyanalyzer-server-{version}/tomcat/webapps/saiku/WEB-INF/lib*.
   * For KyAnalyzer with version later than 2.1.3, running the startup script will download and install the jar package automatically.
   * If no public network is available, a user is required to download the jar from [mondrian-kylin-2.0.jar](https://github.com/Kyligence/kylin-mondrian/blob/master/build/mondrian-kylin-2.0.jar), and copy it to the folder *kyanalyzer-server-{version}/tomcat/webapps/saiku/WEB-INF/lib*.

4. Edit *kyanalyzer.properties* under *kyanalyzer-server-{version}/conf*, and set KAP host and KAP port. *kap.host* represents KAP's IP (default value: localhost) and *kap.port* represents KAP's app port (default port: 7070). And you can configure *mondrian.properties* by referring to *conf/mondrian.properties.template*. (Note: we have moved 'kap.host' and 'kap.port' to *kyanalyzer.properties* since KAP V2.2, and also add *mondrian.properties* to *kyanalyzer-server/conf/*.)

### Startup
Run the following script under *kyanalyzer-{version}* folder to start KyAnalyzer. The default port is 8080. 

   ```sh start-analyzer.sh```

When the server is started, please visit http://{hostname}:8080. If you want to stop the server, please run it under kyanalyzer-{version} folder.

   ```sh stop-analyzer.sh```

If the server is not started normally, please check the logs under tomcat/logs for details. You can check if there is port conflit through *tomcat/logs/catalina.out* to make sure there's no port conflict. To fix the port conflict, find config items as below:

```$xslt
<Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
```
And modify the port to a un-used one.

**Files under root directory:**

![Files under Root Directory](images/server_dir.png)

### Upgrade
KyAnalyzer will save the data under the directories: *repository* and *data*. Suppose you are upgrading KyAnalyzer from KyAnalyzer-1 to KyAnalyzer-2, upgrading steps are as below:

* Back up KyAnalyzer-1's metadata

  + Under KyAnalyzer-1's install directory, create a backup folder, and run the command `mkdir backup`
  + Run the command `cp -r data repository ./backup/` to backup the metadata to the folder *backup*

* Restore the metadata in KyAnalyzer-2 to KyAnalyzer-1

  + Install a new KyAnalyzer as KyAnalyzer-2, and suppose KyAnalyzer-2 and KyAnalyzer-1's folders are {KyAnalyzer-2} and {KyAnalyzer-1} respectively
  + Under KyAnalyzer-2's folder, delete KyAnalyzer-2's metadata folders with the command `rm -rf data repository`
  + Under KyAnalyzer-2's folder, run the command`cp -r ${KyAnalyzer-1}/backup/* ./`

* Notice the default port is 8080, which means that you can visit http://{hostname}:8080 to access KyAnalyzer login page. If you need to change this port, please also config the tomcat/conf/server.xml file or copy and replace the config file of KyAnalyzer-1 to this file.

### Compatibility of KyAnalyzer, KAP and Mondrain-kylin

<table>
    <tr>
    <th>KAP</th>
    <th>KyAnalyzer</th>
    <th>Mondrian-Kylin</th>
    <th>COUNT_DISTINCT</th>
    <th>TOP_N</th>
    <th>Save Calculated Measure</th>
    <th>Normal Query</th>
    <th></th>
    </tr>
    <tr>
        <td>2.0</td>
        <td>&gt;=2.1</td>
        <td>1.0</td>
        <td>❎</td>
        <td>❎</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr>
    <tr>
        <td>2.0</td>
        <td>&gt;=2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>❎</td>
        <td>❎</td>
        <td>❎</td>
        <td></td>
    </tr>
    <tr>
        <td>&gt;=2.1</td>
        <td>&gt;=2.1</td>
        <td>1.0</td>
        <td>❎</td>
        <td>❎</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr> 
    <tr>
        <td>&gt;=2.1</td>
        <td>&gt;=2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>✅</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr>
    <tr>
        <td>&gt;=2.4.0</td>
        <td>&gt;=2.4.0</td>
        <td>2.0</td>
        <td>✅</td>
        <td>✅</td>
        <td>✅</td>
        <td>✅</td>
        <td>Recommended</td>
    </tr>
</table>

### Compatibility of KyAnalyzer, Apache Kylin and Mondrain-kylin

<table>
    <tr>
    <th>Apache Kylin</th>
    <th>KyAnalyzer</th>
    <th>Mondrian-Kylin</th>
    <th>COUNT_DISTINCT</th>
    <th>TOP_N</th>
    <th>Save Calculated Measure</th>
    <th>Normal Query</th>
    <th></th>
    </tr>
    <tr>
        <td>ALL</td>
        <td>&gt;=2.1</td>
        <td>1.0</td>
        <td>❎</td>
        <td>❎</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr>
    <tr>
        <td>&lt;1.5.4.1</td>
        <td>&gt;=2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>❎</td>
        <td>❎</td>
        <td>❎</td>
        <td></td>
    </tr>
    <tr>
        <td>1.5.4.1</td>
        <td>&gt;=2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>❎</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr> 
    <tr>
        <td>&gt;1.5.4.1</td>
        <td>&gt;=2.1</td>
        <td>1.1</td>
        <td>✅</td>
        <td>✅</td>
        <td>❎</td>
        <td>✅</td>
        <td></td>
    </tr>
    <tr>
        <td>&gt;2.0.0</td>
        <td>&gt;=2.4.0</td>
        <td>2.0</td>
        <td>✅</td>
        <td>✅</td>
        <td>✅</td>
        <td>✅</td>
        <td>Recommended</td>
    </tr>  
</table>

### Authentication
KyAnalyzer authenticates a user at KAP side, so you can use KAP user and password to login. Users with *Admin* Role in KAP are also *Admin* in KyAnalyzer.
![KyAnalyzer Login](images/analyzer_login.png)

### Sync Cube
Users may synchronize a Cube from KAP. In doing so, a schema file and a datasource file for each cube from KAP will be created in KyAnalyzer by default. KyAnalyzer automates the workflow, so a user doesn't need to write the schema manually.
Click **Sync Cubes From Kylin** on the left side, then the right panel will list all projects from KAP.

![Sync Cubes From Kylin](images/admin_sync.png)

Click the green button **Sync Cubes From Kylin** after selecting a project, then KyAnalyzer will create a schema file and a datasource file for all cubes under this project with status of **READY**.

![Sync Cubes Results](images/sync_done_tip.png)

The schema could be edited online in KyAnalyzer. Usually, it does not need to be modified.

### Query Permission to Project/Table/Cell-level 
User's query permission to a project and cubes in KyAnalyzer are consistent with KAP. For more details, please refer to [Manage Permissions](../security/acl.en.md).  

User's data access to table/row/column in KyAnalyzer would be consistent with KAP. Except that in KyAnalyzer V2.5.0, a user without query permission to a table (table-level access has been removed for a table) cannot sync any Cube that using this table. For details regarding the data access control in KAP, please refer to [Data Access Control](../security/data_access_control.en.md). 

### New a Query
Click the button **New Query** at the navigation bar, click the button **Refresh** to fetch the latest metadata, and then select the cube to query under the **Cubes** dropdown menu. Now all dimensions and measures are shown. In the report design area, here are four panels: **Measures**, **Columns**, **Rows**, and **Filter**. Users could drag measures into **Measures** panel, and dimensions into **Columns** and **Rows** panels.

![New a Query](images/cube_refresh.png)

### Data Visulization
KyAnalyzer supports many types of data visulization, such as table, bar, stacked bar, line chart, area chart, heat grid, tree map, pie chart, dot chart, and water fall chart. Here we illustrate some examples.

#### Bar Chart
![Bar Chart](images/bar_chart.png)

#### Pie Chart
![Pie Chart](images/pie_chart.png)

#### Pivotal Table
![Pivotal Table](images/pivotal_table.png)

#### COUNT DISTINCT
![Count Distinct](images/count_distinct.png)

#### Hierarchy
In KyAnalyzer, when you synchronize a Cube, you also synchronize the hierarchy information on the Cube to KyAnalyzer. The benefit is that you may be able to perform Drill-down and Roll-up on the Hierarchy dimension.
![Hierarchy](images/hierarchy_table_en.png)



