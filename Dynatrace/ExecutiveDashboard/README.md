<img align="center" src=../../Liquibase_logo_horizontal_WHITE.png>

# Executive Dashboard

This dashboard consists of the following tiles. In order to power the tiles in your environment you will need to customize the `filter` command with matcher values that exist within your environment. (I.E. Change the value of `log.source` to match your filepath and the value of `dt.entity.host` to match the host that's sending the log.)   
<table width="100%">
    <tr>
        <td> <b>Tile</b> </td>
        <td> <b>Sample</b> </td>
    </tr>
    <tr>
        <td valign="top">
            <b>TOTAL DEPLOYMENTS</b><br>Visualization: <b>Single Value</b><br>DQL query: 
<pre>
fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| filter liquibase.deploymentId != ""
| summarize countDistinctExact(liquibase.deploymentId)
</pre>
        </td>
        <td> <img src=img/TotalDeployments.png width="400"> </td>
    </tr>
    <tr>
        <td valign="top">
            <b>ENVIRONMENT DEPLOYMENTS</b><br>Visualization: <b>Single Value</b><br> DQL query: 
<pre>
fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| filter liquibase.deploymentId != ""
| fieldsAdd endsWith(liquibase.liquibaseTargetUrl, "dev"), alias:liquibase.dev
| filter liquibase.dev == true 
| summarize countDistinctExact(liquibase.deploymentId)
</pre>
            Note: <ul>
                <li>For Dev deployments use `liquibaseTargetUrl="dev"` 
                <li>For Test deployments use `liquibaseTargetUrl="test"` 
                <li>For Stage deployments, use `liquibaseTargetUrl="stag"` 
                <li>For Prod deployments use `liquibaseTargetUrl="prod"`
                <li>For any custom environments, use the string after `:` at the end of the target URL 
        </td>
        <td> <img src=img/DevDeployments.png width="400"> </td>
    </tr>
    <tr>
        <td valign="top">
            <b>SUCCESS/FAILURE RATE</b><br>Visualization: <b>Donut</b><br>DQL query: 
<pre>
fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| filter liquibase.deploymentOutcome != "NOOP"
| summarize count(), by:liquibase.deploymentOutcome 
        </td>
        <td> <img src=img/SuccessFailure.png width="400"> </td>
    </tr>
    <tr>
        <td valign="top">
            <b>DEPLOYMENT FREQUENCY</b><br>Visualization: <b>Area</b><br>DQL query: 
<pre>
fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| filter liquibase.deploymentOutcome != "NOOP"
| fields liquibase.timestamp, liquibase.deploymentOutcome
| fieldsAdd toTimestamp(liquibase.timestamp), alias: liquibase.timestamp
| makeTimeseries count(), 
  by: liquibase.deploymentOutcome,
  time: liquibase.timestamp
</pre> 
        </td>
        <td> <img src=img/DeploymentFrequency.png width="400"> </td>
    </tr>
    <tr>
        <td valign="top">
           <b>LEAD TIME TO CHANGE</b><br>Visualization: <b>Single Value</b><br>DQL query: 
<pre>
fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| filter liquibase.changesetOperationStart != "" AND liquibase.deploymentId != "" AND liquibase.level == "INFO" AND liquibase.changesetId != ""
| fieldsAdd endsWith(liquibase.liquibaseTargetUrl, "dev"), alias:liquibase.dev,
  endsWith(liquibase.liquibaseTargetUrl, "prod"), alias:liquibase.prod
| filter liquibase.dev == true 
| dedup liquibase.changesetId, liquibase.liquibaseTargetUrl
| fieldsAdd  liquibase.timestamp = toTimestamp(liquibase.timestamp)
| fields liquibase.timestamp, alias:liquibase.timestamp.start, liquibase.changesetId
| join [fetch logs
  | filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
  | parse content, "JSON:liquibase"
  | fieldsFlatten liquibase
  | filter liquibase.changesetOperationStart != "" AND liquibase.deploymentId != "" AND liquibase.level == "INFO" AND liquibase.changesetId != ""
  | fieldsAdd endsWith(liquibase.liquibaseTargetUrl, "dev"), alias:liquibase.dev,
    endsWith(liquibase.liquibaseTargetUrl, "prod"), alias:liquibase.prod
  | filter liquibase.prod == true
  | dedup liquibase.changesetId, liquibase.liquibaseTargetUrl
  | fieldsAdd  liquibase.timestamp = toTimestamp(liquibase.timestamp)
  | fields liquibase.timestamp, alias:liquibase.timestamp.end, liquibase.changesetId
  ],
  on: {liquibase.changesetId},
  fields: {liquibase.timestamp.end, liquibase.changesetId}
| fieldsAdd duration = liquibase.timestamp.end - liquibase.timestamp.start
| summarize avg(duration)
</pre> 
        </td>
        <td> <img src=img/LeadTimeToChange.png width="400"> </td>
    </tr>
    <tr>
        <td valign="top">
            <b>Releases by Top Apps</b><br>Visualization: <b>Donut</b><br>DQL query: 
<pre>
fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| filter liquibase.deploymentId != ""
| summarize count(), by: liquibase.commandLabelFilter
</pre>
        </td>
        <td> <img src=img/ReleasesByTopApps.png width="400"> </td>
    </tr>
    <tr>
        <td valign="top">
            <b>Rollbacks by Apps</b><br>Visualization: <b>Bar Chart</b><br>DQL query: 
<pre>
fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| filter liquibase.deploymentId != "" 
| fieldsAdd startsWith(liquibase.liquibaseCommandName, "rollback"), alias:liquibase.rollback
| filter liquibase.rollback == true 
| fields liquibase.timestamp, liquibase.commandLabelFilter
| fieldsAdd toTimestamp(liquibase.timestamp), alias: liquibase.timestamp
| makeTimeseries count(), 
  interval: 1h,
  by: {liquibase.commandLabelFilter},
  time: liquibase.timestamp
</pre>
        </td>
        <td> <img src=img/RollbacksByApps.png width="400"> </td>
    </tr>
    <tr>
        <td valign="top">
            <b>MEAN TIME TO RESOLUTION</b><br>Visualization: <b>Single Value</b><br>DQL query: 
<pre>
fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| fields liquibase.timestamp, liquibase.deploymentOutcome, liquibase.deploymentId, liquibase.liquibaseTargetUrl
| filter liquibase.deploymentOutcome == "failure"
| dedup liquibase.deploymentId, liquibase.liquibaseTargetUrl
| fieldsAdd  liquibase.timestamp = toTimestamp(liquibase.timestamp)
| fields liquibase.timestamp, alias:liquibase.timestamp.start, liquibase.deploymentId
| join [fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| fields liquibase.timestamp, liquibase.deploymentOutcome, liquibase.deploymentId, liquibase.liquibaseTargetUrl
| filter liquibase.deploymentOutcome == "success" 
| dedup liquibase.deploymentId, liquibase.liquibaseTargetUrl
| fieldsAdd  liquibase.timestamp = toTimestamp(liquibase.timestamp)
| fields liquibase.timestamp, alias:liquibase.timestamp.end, liquibase.deploymentId
  ],
  on: {liquibase.deploymentId},
  fields: {liquibase.timestamp.end, liquibase.deploymentId}
| fieldsAdd duration = liquibase.timestamp.end - liquibase.timestamp.start
| summarize avg(duration)
</pre>
        </td>
        <td> <img src=img/MTTR.png width="400"> </td>
    </tr>
    <tr>
        <td valign="top">
            <b>Latest Checks Triggered</b><br>Visualization: <b>Table</b><br>DQL query: 
<pre>
fetch logs
| filter matchesValue(log.source, "C:\\Users\\Administrator\\Documents\\mylogfile.log") and matchesValue(dt.entity.host, "HOST-8ECAB10AF161550E")
| parse content, "JSON:liquibase"
| fieldsFlatten liquibase
| filter liquibase.qualityChecks.changelogChecks.issues.checkSeverity == "INFO"
| fields liquibase.timestamp, alias:timestamp, liquibase.qualityChecks.changelogChecks.issues.checkSeverity, alias:checkSeverity, liquibase.qualityChecks.changelogChecks.issues.checkShortName, alias:checkShortName, liquibase.qualityChecks.changelogChecks.issues.changesetId, alias:changesetId, liquibase.qualityChecks.changelogChecks.issues.changesetAuthor, alias:changesetAuthor, liquibase.qualityChecks.changelogChecks.issues.changesetFilePath, alias:changesetFilePath
| sort -timestamp
| limit 5
</pre>
        </td>
        <td> <img src=img/LatestChecksTriggered.png width="400"> </td>
    </tr>
</table>