# DATADOG DASHBOARDS 

*** WORK IN PROGRESS ***

This page contains JSON exports of two Datadog dashboards: 

* Executive Dashboard
* Database Dashboard

## Executive Dashboard

This dashboard consists of the following widgets:
| Widget | Sample |
| ------ | ------ |
|<b>TOTAL DEPLOYMENTS</b><br>Visualization: Query Value<br>Data:<li>`Logs: Show (Deployment ID (@deploymentId))` | <img src=TotalDeployments.png width="300"> |
|<b>SUCCESS/FAILURE</b><br>Visualization: Pie Chart<br>Data:<li>`Logs: Show Count unique of (Deployment ID (@deploymentId)) by (Deployment Outcome (@deploymentOutcome)) limit to top 10` | <img src=Success_Failure.png width="300"> |
|<b>DEPLOYMENTS BY ENVIRONMENT</b><br>Visualization: Top List<br>Data:<li>`Logs: Show Count of * by (Environment (@contextCommandFilter)) limit to top 10` | <img src=DeploymentsbyEnvironment.png width="500"> |
|<b>TOTAL APPLICATIONS</b><br>Visualization: Query Value<br>Data:<li>`Logs: Show (Application (@commandLabelFilter))` | <img src=TotalApplications.png width="300"> |
|<b>APPLICATION DEPLOYMENTS</b><br>Visualization: Tree Map<br>Data:<li>`Logs: Show Count unique of Deployment ID (@deploymentId) by Application (@commandLabelFilter) limit to top 15` | <img src=ApplicationDeployments.png width="300"> |
|<b>ROLLBACK BY APPLICATION</b><br>Visualization: Top List<br>Data:<li>`Logs: Show Count of * by (Application (@commandLabelFilter)) limit to top 10` | <img src=RollbacksbyApplication.png width="300"> |
|<b>LAST 25 DEPLOYMENTS</b><br>Visualization: Table<br>Data:<li>`Logs: Show Count of * by (Deployment ID (@deploymentId) Application (@commandLabelFilter) Environment (@commandContextFilter)) limit to top 10` | <img src=Last25Deployments.png width="300"> |

## Database Dashboard

