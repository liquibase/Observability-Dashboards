# Executive Dashboard

This dashboard consists of the following widgets:
| Widget |
| ------ |
|<b>TOTAL DEPLOYMENTS</b><br>Visualization: <b>Stat</b><br>SQL query: `fields @timestamp, deploymentId \| filter deploymentId != "" \| stats count_distinct(deploymentId)` 
|<b>ENVIRONMENT DEPLOYMENTS</b><br>Visualization: <b>Stat</b><br> SQL query: `fields @timestamp, deploymentId \| filter deploymentId != "" and liquibaseTargetUrl like /dev/\| stats count_distinct(deploymentId)`<br> Note: <ul><li>For Dev deployments use `liquibaseTargetUrl like /dev/` <li>For Test deployments use `liquibaseTargetUrl like /test/` <li>For Stage deployments, use `liquibaseTargetUrl like /stage/` <li>For Prod deployments use `liquibaseTargetUrl like /prod/` 
|<b>SUCCESS/FAILURE RATE</b><br>Visualization: <b>Pie chart</b><br>SQL query: `fields @timestamp, @message \| parse @message /deploymentOutcome["\s]*:["\s]*"(?<outcome>[^"]*)/ \| filter outcome = "success" \| stats count() as Successes` 
|<b>ROLLBACKS</b><br>Visualization: <b>Stat</b><br>SQL query: `fields @timestamp, deploymentId \| filter deploymentId != "" and liquibaseCommandName like /(?i).*rollback.*/ \| stats count_distinct(deploymentId) as rollbackDeployments` 
